# Capacité : Créer Service Métier

## 🎯 Contexte
Un service contient la logique métier isolée de la présentation HTTP et de la base de données. Réutilisable, testable, et indépendante.

---

## 📋 Avant de Coder

- [ ] Domaine métier identifié (User, Order, Invoice, etc.)
- [ ] Opérations métier listées (create, update, publish, etc.)
- [ ] Dépendances (modèles, autres services) identifiées
- [ ] Exceptions métier prévues

---

## 🔧 Créer un Service

### Étape 1 : Créer le Dossier
Créer `app/Services/` s'il n'existe pas.

### Étape 2 : Créer la Classe
Fichier : `app/Services/UserService.php`

```php
namespace App\Services;

use App\Models\User;

class UserService {
    // Méthodes métier ici
}
```

### Étape 3 : Implémenter CRUD Basique
```php
public function create(array $data): User {
    return User::create($data);
}

public function update(User $user, array $data): User {
    $user->update($data);
    return $user;
}

public function delete(User $user): bool {
    return $user->delete();
}

public function getById(int $id): ?User {
    return User::find($id);
}

public function getAll(): Collection {
    return User::all();
}
```

### Étape 4 : Ajouter Logique Métier
```php
public function activateUser(User $user): User {
    if ($user->email_verified_at === null) {
        throw new Exception('Email non vérifié');
    }
    $user->update(['is_active' => true]);
    return $user;
}

public function suspendUser(User $user, string $reason): User {
    $user->update(['suspended_at' => now(), 'suspension_reason' => $reason]);
    // Notifier l'user, logger, etc.
    return $user;
}
```

### Étape 5 : Utiliser dans le Contrôleur
```php
class UserController {
    public function __construct(private UserService $service) {}
    
    public function store(StoreUserRequest $request) {
        $user = $this->service->create($request->validated());
        return redirect()->route('users.show', $user);
    }
}
```

---

## 🏗️ Structure d'un Service Complet

```php
namespace App\Services;

use App\Models\User;
use Illuminate\Database\Eloquent\Collection;

class UserService {
    
    // Opération de création
    public function create(array $data): User {
        return User::create($data);
    }
    
    // Opération de modification
    public function update(User $user, array $data): User {
        $user->update($data);
        return $user;
    }
    
    // Opération spécifique métier
    public function upgradeToAdmin(User $user): User {
        $user->update(['role' => 'admin']);
        event(new UserPromoted($user));
        return $user;
    }
    
    // Getter utile
    public function getActiveUsers(): Collection {
        return User::where('is_active', true)->get();
    }
}
```

---

## 🔒 Gestion des Exceptions

**Lancer des exceptions claires**
```php
public function transfer(Order $order, Warehouse $warehouse) {
    if ($warehouse->isFull()) {
        throw new Exception('Entrepôt plein');
    }
    
    if ($order->isPaid() === false) {
        throw new Exception('Commande non payée');
    }
    
    // Logique de transfert...
}
```

**Capturer dans le contrôleur**
```php
public function transfer(Request $request) {
    try {
        $this->service->transfer($order, $warehouse);
        return redirect()->back()->with('success', 'Transféré');
    } catch (Exception $e) {
        return redirect()->back()->with('error', $e->getMessage());
    }
}
```

---

## 💡 Patterns Courants

**Transactions pour Opérations Critiques**
```php
use Illuminate\Support\Facades\DB;

public function processPayment(Order $order, Payment $payment): void {
    DB::transaction(function () use ($order, $payment) {
        $payment->save();
        $order->update(['status' => 'paid']);
        // Tous les changements ou rien
    });
}
```

**Événements pour Notifications**
```php
public function create(array $data): User {
    $user = User::create($data);
    event(new UserCreated($user));  // Notifier les auditeurs
    return $user;
}
```

**Utiliser des MiniServices**
```php
class OrderService {
    public function __construct(
        private EmailService $mailer,
        private PaymentService $payment
    ) {}
    
    public function checkout(Order $order) {
        $this->payment->charge($order);
        $this->mailer->sendConfirmation($order);
    }
}
```

---

## ⚠️ Erreurs Courantes

- ❌ Accès direct à `Auth::user()` → Injecter user en paramètre
- ❌ Requête HTTP directe → Déléguer à un contrôleur
- ❌ Pas de retour typé → Retourner Model ou DTO
- ❌ Logique mélangée → Séparer en services distincts

---

## ✅ Validation Avant Livraison

- [ ] Service créé dans `app/Services/`
- [ ] CRUD basique implémenté
- [ ] Logique métier isolée
- [ ] Exceptions lancées avec messages clairs
- [ ] Utilisé depuis le contrôleur
- [ ] Testable indépendamment
- [ ] Pas de dépendance à HTTP/Blade
