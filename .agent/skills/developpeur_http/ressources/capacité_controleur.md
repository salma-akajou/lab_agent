# Capacité : Implémenter Contrôleur HTTP

## 🎯 Contexte
Les contrôleurs reçoivent les requêtes HTTP, coordonnent les opérations, et retournent des réponses. Ils doivent rester **minces** et déléguer la logique métier.

---

## 📋 Avant de Développer

- [ ] Routes définies pour la ressource
- [ ] FormRequests créées pour validation
- [ ] Service métier identifié
- [ ] Actions CRUD à implémenter

---

## 🔧 Créer un Contrôleur

### Étape 1 : Générer la Classe
```bash
php artisan make:controller UserController
# Crée: app/Http/Controllers/UserController.php
```

### Étape 2 : Injecter les Dépendances
```php
class UserController extends Controller {
    
    public function __construct(private UserService $service) {}
    
    // Accès à $this->service dans toutes les méthodes
}
```

### Étape 3 : Implémenter les 7 Méthodes CRUD

**Index** (Lister)
```php
public function index() {
    $users = User::all();
    return view('users.index', compact('users'));
}
```

**Create** (Formulaire)
```php
public function create() {
    return view('users.create');
}
```

**Store** (Créer)
```php
public function store(StoreUserRequest $request) {
    $user = $this->service->create($request->validated());
    return redirect()->route('users.show', $user)->with('success', 'Créé!');
}
```

**Show** (Détail)
```php
public function show(User $user) {
    return view('users.show', compact('user'));
}
```

**Edit** (Formulaire édition)
```php
public function edit(User $user) {
    return view('users.edit', compact('user'));
}
```

**Update** (Modifier)
```php
public function update(UpdateUserRequest $request, User $user) {
    $this->service->update($user, $request->validated());
    return redirect()->route('users.show', $user)->with('success', 'Modifié!');
}
```

**Destroy** (Supprimer)
```php
public function destroy(User $user) {
    $this->service->delete($user);
    return redirect()->route('users.index')->with('success', 'Supprimé!');
}
```

### Étape 4 : Ajouter Autorisations
```php
public function edit(User $user) {
    $this->authorize('update', $user);  // Vérifier la policy
    return view('users.edit', compact('user'));
}
```

### Étape 5 : Tester
```bash
php artisan route:list | grep user
```

---

## 🏗️ Structure Type Complète

```php
namespace App\Http\Controllers;

use App\Models\User;
use App\Services\UserService;
use App\Http\Requests\StoreUserRequest;
use App\Http\Requests\UpdateUserRequest;

class UserController extends Controller {
    
    public function __construct(private UserService $service) {}

    // 7 méthodes CRUD ici...
}
```

---

## 🚫 Ce Qu'il NE Faut PAS Faire

❌ **Logique métier en contrôleur**
```php
// BAD
public function store(Request $request) {
    $user = new User();
    $user->name = $request->name;
    $user->email = $request->email;
    if ($user->email_ends_with_@domain) { ... }  // Logique!
    $user->save();
}

// GOOD
public function store(StoreUserRequest $request) {
    $user = $this->service->create($request->validated());
}
```

❌ **SQL brut**
```php
// BAD
DB::select('SELECT * FROM users WHERE ...');

// GOOD
User::where('...')->get();
```

❌ **Pas d'autorisation**
```php
// BAD
public function delete(User $user) {
    $user->delete();  // Pas de vérification!
}

// GOOD
public function delete(User $user) {
    $this->authorize('delete', $user);
    $user->delete();
}
```

---

## 💡 Patterns Utiles

**Retourner du JSON (API)**
```php
public function show(User $user) {
    return response()->json($user);
}
```

**Répondre avec statut personnalisé**
```php
return response()->json(['error' => 'Not found'], 404);
```

**Redirect avec message flash**
```php
return redirect()->back()->with('error', 'Impossible!');
```

---

## ✅ Validation Avant Livraison

- [ ] 7 méthodes CRUD implémentées
- [ ] Services injectés et utilisés
- [ ] FormRequests appliquées
- [ ] Autorisations vérifiées
- [ ] Pas de logique métier directe
- [ ] Redirections et flash messages OK
- [ ] Testable avec requêtes HTTP
