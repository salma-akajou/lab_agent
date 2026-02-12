# Capacité : Valider avec FormRequest

## 🎯 Contexte
Les FormRequests centralisent la validation des entéees utilisateurs, séparant les règles du contrôleur et garantissant la sécurité des données.

---

## 📋 Avant de Coder

- [ ] Champs à valider identifiés
- [ ] Règles de validation définies
- [ ] Messages d'erreur préparés
- [ ] Autorisations à vérifier

---

## 🔧 Créer une FormRequest

### Étape 1 : Générer la Classe
```bash
php artisan make:request StoreUserRequest
# Crée: app/Http/Requests/StoreUserRequest.php
```

### Étape 2 : Implémenter authorize()
```php
public function authorize(): bool {
    return Auth::check();  // L'utilisateur est connecté?
}
```

Retourner `false` refusera la requête (403 Forbidden).

### Étape 3 : Implémenter rules()
```php
public function rules(): array {
    return [
        'name' => 'required|string|max:255',
        'email' => 'required|email|unique:users',
        'password' => 'required|min:8|confirmed',
    ];
}
```

### Étape 4 : Ajouter des Messages Personnalisés
```php
public function messages(): array {
    return [
        'name.required' => 'Le nom est obligatoire.',
        'email.unique' => 'Cet email est déjà utilisé.',
        'password.confirmed' => 'Les mots de passe ne correspondent pas.',
    ];
}
```

### Étape 5 : Utiliser dans le Contrôleur
```php
public function store(StoreUserRequest $request) {
    $validated = $request->validated();  // Données validées
    $user = User::create($validated);
    return redirect()->route('users.show', $user);
}
```

---

## 📚 Règles Courantes

| Règle | Usage | Exemple |
|-------|-------|---------|
| `required` | Obligatoire | `name => 'required'` |
| `string` | Type chaîne | `email => 'string'` |
| `email` | Format email | `email => 'email'` |
| `max:255` | Longueur max | `name => 'max:255'` |
| `min:8` | Longueur min | `password => 'min:8'` |
| `unique:table` | Unique en BDD | `email => 'unique:users'` |
| `exists:table` | Existe en BDD | `user_id => 'exists:users'` |
| `confirmed` | Double saisie | `password => 'confirmed'` |
| `numeric` | Nombre | `age => 'numeric'` |
| `date` | Format date | `birth_date => 'date'` |
| `nullable` | Optionnel | `phone => 'nullable\|phone'` |

---

## 🔒 Autorisation & Sécurité

**Vérifier un droit spécifique**
```php
public function authorize(): bool {
    $user = Auth::user();
    $post = $this->route('post');
    return $user->can('update', $post);  // Policy
}
```

**Refuser si non admin**
```php
public function authorize(): bool {
    return Auth::user()->isAdmin();
}
```

---

## 🧹 Nettoyer les Données

**Avant validation**
```php
protected function prepareForValidation() {
    $this->merge([
        'slug' => Str::slug($this->name),
    ]);
}
```

**Après validation**
```php
protected function passedValidation() {
    $this->merge([
        'email' => strtolower($this->email),
    ]);
}
```

---

## 💡 Patterns Courants

**Email Unique Sauf Lui-même**
```php
'email' => 'unique:users,email,' . $this->user_id,
```

**Validation Conditionnelle**
```php
'phone' => 'required_if:country,FR',  // Requis si country=FR
'fax' => 'nullable|required_without:phone',  // Requis si pas phone
```

**Règle Customisée**
```php
'age' => ['required', 'numeric', Rule::in([18, 21, 65])],
```

---

## ⚠️ Erreurs Courantes

- ❌ Sauter la validation → Données sales
- ❌ Messages non localisés → Mauvaise UX
- ❌ Autorisation absente → Sécurité compromise
- ❌ Règles complexes → Déléguer au service

---

## ✅ Validation Avant Livraison

- [ ] FormRequest créée et testée
- [ ] Autorisation implémentée
- [ ] Règles pour tous les champs
- [ ] Messages d'erreur clairs (FR)
- [ ] Données validées avant traitement
- [ ] Erreurs s'affichent dans la vue
