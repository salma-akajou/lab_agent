# Capacité : Implémenter Règles Métier

## 🎯 Contexte
Les règles métier encapsulent les contraintes, validations complexes et états du domaine. Elles doivent être testées et documentées.

---

## 📋 Identifier les Règles

Avant de coder, lister les règles:
- Qui peut faire quoi? (rôles, permissions)
- Quand peut-on faire une action? (états, conditions)
- Quels calculs métier? (prix, commissions)
- Quel impact sur les données? (cascades, atomicité)

---

## 🔧 Implémenter les Règles

### Étape 1 : Créer des Méthodes Explicites

Nommer clairement chaque règle:
```php
private function validateOrderEligibility(User $user, Order $order): void {
    // Validation...
}

private function canUserApproveExpense(User $user, Expense $expense): bool {
    // Vérification...
}

private function calculateDeliveryFee(Order $order): float {
    // Calcul...
}
```

### Étape 2 : Utiliser dans les Services Publiques
```php
public function checkout(User $user, Order $order): void {
    $this->validateOrderEligibility($user, $order);
    $fee = $this->calculateDeliveryFee($order);
    
    DB::transaction(function () use ($order, $fee) {
        $order->update(['total' => $order->subtotal + $fee]);
        $order->markAsProcessed();
    });
}
```

### Étape 3 : Lever des Exceptions Claires
```php
private function validateOrderEligibility(User $user, Order $order): void {
    if ($user->isSuspended()) {
        throw new Exception('Compte suspendu');
    }
    
    if ($order->subtotal < 10) {
        throw new Exception('Montant minimum 10€');
    }
    
    if ($order->items->isEmpty()) {
        throw new Exception('Panier vide');
    }
}
```

### Étape 4 : Tester les Cas Limites

Penser à tous les cas:
```php
// Cas nominal
$this->service->checkout($user, $order);

// Cas limites (errors)
$this->expectException(Exception::class);
$this->service->checkout($suspendedUser, $order);

$this->expectException(Exception::class);
$this->service->checkout($user, $invalidOrder);
```

---

## 💡 Patterns de Règles Courants

**État Machine (Transitions)**
```php
private function canTransitionTo(Order $order, string $newStatus): bool {
    $transitions = [
        'pending' => ['processing', 'cancelled'],
        'processing' => ['shipped', 'cancelled'],
        'shipped' => ['delivered', 'returned'],
    ];
    
    return in_array($newStatus, $transitions[$order->status] ?? []);
}

public function updateStatus(Order $order, string $newStatus): void {
    if (! $this->canTransitionTo($order, $newStatus)) {
        throw new Exception("Transition invalide: {$order->status} → {$newStatus}");
    }
    $order->update(['status' => $newStatus]);
}
```

**Vérification Hiérarchique**
```php
private function hasApprovalRight(User $user, Expense $expense): bool {
    if ($user->isAdmin()) {
        return true;  // Admin approuve tout
    }
    
    if ($user->isManager() && $expense->amount < 1000) {
        return true;  // Manager approuve < 1000€
    }
    
    return false;
}
```

**Calculs avec Seuils**
```php
private function calculateShippingCost(Order $order): float {
    // Gratuit si > 100€
    if ($order->subtotal >= 100) {
        return 0;
    }
    
    // 10€ pour < 50€
    if ($order->subtotal < 50) {
        return 10;
    }
    
    // 5€ pour 50€-100€
    return 5;
}
```

**Validations Composées**
```php
private function canPublishArticle(Article $article): bool {
    return $article->isDraft()
        && $article->hasTitle()
        && $article->hasContent()
        && $article->author->isVerified();
}
```

---

## 📐 Exemple Full Scenario

```php
class InvoiceService {
    
    public function generateInvoice(Order $order): Invoice {
        $this->validateOrderReadyForBilling($order);
        
        $invoice = Invoice::create([
            'order_id' => $order->id,
            'amount' => $this->calculateFinalAmount($order),
            'due_date' => $this->calculateDueDate($order),
        ]);
        
        event(new InvoiceGenerated($invoice));
        return $invoice;
    }
    
    // Règles métier privées
    
    private function validateOrderReadyForBilling(Order $order): void {
        if (! $order->isPaid()) {
            throw new Exception('Commande non payée');
        }
        if ($order->hasInvoice()) {
            throw new Exception('Facture déjà générée');
        }
        if ($order->items->isEmpty()) {
            throw new Exception('Pas d\'articles');
        }
    }
    
    private function calculateFinalAmount(Order $order): float {
        $subtotal = $order->items->sum('total');
        $tax = $subtotal * 0.20;
        return $subtotal + $tax;
    }
    
    private function calculateDueDate(Order $order): Carbon {
        // 30 jours pour B2B, immédiat pour B2C
        $days = $order->customer->isB2B() ? 30 : 0;
        return now()->addDays($days);
    }
}
```

---

## ⚠️ Anti-Patterns à Éviter

- ❌ Répéter les validations partout → Centraliser
- ❌ Silent failures (erreurs ignorées) → Lancer exceptions
- ❌ Logique implicite → Noms clairs et commentaires
- ❌ Mélanger UI et métier → Séparer les couches

---

## ✅ Validation Finale

- [ ] Règles métier identifiées et documentées
- [ ] Validations implémentées comme méthodes privées
- [ ] Exceptions lancées avec messages clairs
- [ ] Cas limites testés (3+ cas par règle)
- [ ] Transactions si opérations liées
- [ ] Code lisible et maintenable
- [ ] Tests unitaires couvrant les règles
