# Capacité : Gérer les Routes HTTP

## 🎯 Contexte
Les routes mappent les URLs aux contrôleurs. Une structure claire et RESTful facilite la navigation du code et de l'API.

---

## 📋 Avant de Coder

- [ ] Ressources identifiées (User, Post, Product, etc.)
- [ ] Actions CRUD planifiées
- [ ] Groupes de routes identifiés (auth, admin, api)
- [ ] Middlewares nécessaires définis

---

## 🔧 Mettre en Place les Routes

### Étape 1 : Ouvrir le Fichier Routes
- Pour web : `routes/web.php`
- Pour API : `routes/api.php`

### Étape 2 : Créer une Route Simple
```php
Route::get('/users', [UserController::class, 'index'])->name('users.index');
```

Décomposition:
- `GET` = méthode HTTP
- `/users` = URI
- `[UserController::class, 'index']` = contrôleur + méthode
- `->name('users.index')` = nom identifiant

### Étape 3 : Implémenter CRUD Complet
```php
Route::post('/users', [UserController::class, 'store'])->name('users.store');
Route::get('/users/{user}', [UserController::class, 'show'])->name('users.show');
Route::put('/users/{user}', [UserController::class, 'update'])->name('users.update');
Route::delete('/users/{user}', [UserController::class, 'destroy'])->name('users.destroy');
```

### Étape 4 : Regrouper les Routes
```php
Route::middleware('auth')->group(function () {
    Route::resource('posts', PostController::class);
});
```

### Étape 5 : Tester
```bash
php artisan route:list  # Voir toutes les routes
```

---

## 📜 Convention RESTful Standard

| Verbe | URI | Méthode | Usage |
|-------|-----|---------|-------|
| GET | `/users` | `index` | Lister toute |
| POST | `/users` | `store` | Créer |
| GET | `/users/{id}` | `show` | Afficher détail |
| GET | `/users/{id}/edit` | `edit` | Formulaire édition |
| PUT | `/users/{id}` | `update` | Modifier |
| DELETE | `/users/{id}` | `destroy` | Supprimer |

---

## 🏗️ Patterns Organisationnels

**Grouper par Ressource**
```php
Route::controller(UserController::class)->group(function () {
    Route::get('/users', 'index')->name('users.index');
    Route::post('/users', 'store')->name('users.store');
    Route::get('/users/{user}', 'show')->name('users.show');
});
```

**Grouper par Middleware**
```php
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/dashboard', ...);
    Route::post('/posts', ...);
});
```

**Routes Imbriquées (Relations)**
```php
Route::get('/users/{user}/posts', [PostController::class, 'userPosts']);
```

**Routes API Versionnées**
```php
Route::prefix('api/v1')->group(function () {
    Route::apiResource('users', ApiUserController::class);
});
```

---

## 🔐 Sécurité & Bonnes Pratiques

✅ Accès **nommés** via `route('name', $params)` (refactoring facile)  
✅ Groupes middleware pour auth rapide  
✅ URI **minuscules** avec tirets (jamais camelCase)  
✅ Ressources **au pluriel**  
❌ Jamais de logique dans la route  
❌ Jamais de paramètres inutiles

---

## ⚠️ Erreurs Courantes

- ❌ Noms de routes vagues → Use `users.show` pas juste `show`
- ❌ Oublier middleware auth → Routes exposées
- ❌ Routes non nommées → Refactoring difficile
- ❌ Mélanger web et API sans préfixe → Collision

---

## ✅ Validation Finale

- [ ] Toutes les routes nommées
- [ ] Conventions RESTful respectées
- [ ] Middleware appliqué (auth, CSRF, etc.)
- [ ] Testable avec route:list
- [ ] Pas de duplication
- [ ] Suivre la structure du projet
