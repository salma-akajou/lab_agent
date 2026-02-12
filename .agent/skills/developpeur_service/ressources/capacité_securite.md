# Capacité : Définir Policy & Sécurité

## 🎯 Contexte
Les Policies formalisent les règles d'autorisation (qui peut faire quoi sur quelle ressource). Centralisées, elles garantissent la cohérence des droits.

---

## 📋 Avant de Créer Une Policy

- [ ] Ressource et modèle identifiés
- [ ] Rôles/utilisateurs actuels clairement définis
- [ ] Scenarios d'accès listés (voir, créer, modifier, supprimer)
- [ ] Cas spéciaux comptabilisés (admin, propriétaire)

---

## 🔧 Créer et Utiliser une Policy

### Étape 1 : Générer le fichier
```bash
php artisan make:policy PostPolicy --model=Post
# Crée: app/Policies/PostPolicy.php
```

### Étape 2 : Implémenter les Méthodes
```php
namespace App\Policies;

use App\Models\User;
use App\Models\Post;

class PostPolicy {
    
    // Avant tout, laisser passer les admins
    public function before(User $user): bool|null {
        if ($user->isAdmin()) {
            return true;
        }
        return null;  // Continuer aux autres méthodes
    }
    
    public function view(User $user, Post $post): bool {
        return $post->is_published || $user->id === $post->author_id;
    }
    
    public function create(User $user): bool {
        return $user->is_verified;  // Seulement utilisateurs vérifiés
    }
    
    public function update(User $user, Post $post): bool {
        return $user->id === $post->author_id;  // Propriétaire
    }
    
    public function delete(User $user, Post $post): bool {
        return $user->id === $post->author_id;  // Propriétaire
    }
    
    public function restore(User $user, Post $post): bool {
        return $user->id === $post->author_id;  // Propriétaire
    }
}
```

### Étape 3 : Enregistrer la Policy
Fichier: `app/Providers/AuthServiceProvider.php`

```php
protected $policies = [
    Post::class => PostPolicy::class,
];
```

### Étape 4 : Utiliser dans le Contrôleur
```php
class PostController {
    
    public function show(Post $post) {
        // Vérifier la policy "view"
        $this->authorize('view', $post);
        return view('posts.show', compact('post'));
    }
    
    public function edit(Post $post) {
        // Vérifier la policy "update"
        $this->authorize('update', $post);
        return view('posts.edit', compact('post'));
    }
    
    public function destroy(Post $post) {
        $this->authorize('delete', $post);
        $post->delete();
        return redirect()->route('posts.index');
    }
}
```

### Étape 5 : Utiliser dans les Vues
```blade
@can('view', $post)
    <a href="{{ route('posts.show', $post) }}">Voir</a>
@endcan

@can('update', $post)
    <a href="{{ route('posts.edit', $post) }}">Éditer</a>
@endcan

@can('delete', $post)
    <form method="POST" action="{{ route('posts.destroy', $post) }}">
        @csrf @method('DELETE')
        <button type="submit">Supprimer</button>
    </form>
@endcan
```

---

## 📜 Méthodes Standards

| Méthode | Paramétre 2 | Usage |
|---------|-------------|-------|
| `viewAny(User)` | - | Voir toutes? |
| `view(User, Model)` | Ressource | Voir celle-ci? |
| `create(User)` | - | Créer neuve? |
| `update(User, Model)` | Ressource | Modifier celle-ci? |
| `delete(User, Model)` | Ressource | Supprimer celle-ci? |
| `restore(User, Model)` | Soft-deleted | Restaurer celle-ci? |
| `forceDelete(User, Model)` | Soft-deleted | Supprimer définitivement? |

---

## 👤 Patterns Sécurité Courants

**Admin Override**
```php
public function before(User $user): bool|null {
    if ($user->hasRole('admin')) {
        return true;
    }
    return null;  // Continuer...
}
```

**Propriétaire Uniquement**
```php
public function update(User $user, Post $post): bool {
    return $user->id === $post->author_id;
}
```

**Rôles & Permissions**
```php
public function approve(User $user, Expense $expense): bool {
    if ($user->can('approve-all-expenses')) {
        return true;
    }
    // Gestionnaire ne peut approver que ses équipes
    return $user->team_id === $expense->team_id 
        && $user->can('approve-expenses');
}
```

**Condition Métier**
```php
public function refund(User $user, Order $order): bool {
    // Seul le commerce ou user peut refund
    if ($user->isCustomerService() || $user->id === $order->user_id) {
        return $order->isRefundable();  // Et seulement si refundable
    }
    return false;
}
```

---

## 🛡️ Checklist de Sécurité

✅ Toutes les actions sensibles ont une policy  
✅ Admin bypass avec `before()`  
✅ Messages d'erreur sans détails sensibles  
✅ Logging des refus d'accès  
✅ Tests des policy pour tous les cas  
✅ Propriété vérifié avant modification  
✅ Soft-deletes gérés (restore, forceDelete)

---

## ⚠️ Erreurs Courantes

- ❌ Policy non enregistrée → Autorisations ignorées
- ❌ `before()` retourne un bool au lieu de null → Bloque autres règles
- ❌ Oublier un cas (admin, owner) → Fuite de sécurité
- ❌ Pas de test des policies → Bugs en production

---

## ✅ Validation Avant Livraison

- [ ] Policy créée pour chaque ressource protégée
- [ ] Méthodes standards implémentées
- [ ] `before()` pour admin bypass
- [ ] Policy enregistrée dans AuthServiceProvider
- [ ] Vérifications `authorize()` en place
- [ ] Vues utilisent `@can/@cannot`
- [ ] Tests couvrent tous les rôles
- [ ] 403 retourné si non autorisé
