# Capacité : Définir Modèle Eloquent

## 🎯 Contexte
Un modèle Eloquent est la représentation ORM d'une table. Il encapsule la logique d'accès et de manipulation des données, ainsi que les relations avec d'autres modèles.

---

## 📋 Checklist Préalable
- [ ] La migration correspondante est exécutée
- [ ] Les relations identifiées (belongsTo, hasMany, etc.)
- [ ] Les attributs à typer sont connus
- [ ] Les règles d'accès en masse définies

---

## 🔧 Processus Création

### Étape 1 : Générer la Classe
```bash
php artisan make:model User
# Optionnel : avec migration ou factory
# php artisan make:model User -mf
```

Localisation : `app/Models/User.php`

### Étape 2 : Configurer la Protection de Masse
**Obligatoire** : Définir `$fillable` (liste blanche)
```php
protected $fillable = ['name', 'email', 'phone'];
```
Jamais utiliser `$guarded = []` sans protection explicite.

### Étape 3 : Déclarer les Casts
Typage des attributs pour conversion automatique :
```php
protected $casts = [
    'email_verified_at' => 'datetime',
    'is_active' => 'boolean',
    'metadata' => 'json',
];
```

### Étape 4 : Définir les Relations
```php
public function posts() {
    return $this->hasMany(Post::class);
}

public function role() {
    return $this->belongsTo(Role::class);
}
```

### Étape 5 : Tester
```php
$user = User::create(['name' => 'John', 'email' => 'john@test.com']);
$user->posts->count(); // Relation fonctionne ?
```

---

## 🏗️ Structure Complète Minimale

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;

class User extends Model {
    protected $fillable = ['name', 'email'];
    protected $casts = ['email_verified_at' => 'datetime'];
    
    public function posts(): HasMany {
        return $this->hasMany(Post::class);
    }
}
```

---

## 🔗 Types de Relations Courants

| Relation | Syntaxe | Exemple |
|----------|---------|---------|
| Un-à-plusieurs | `hasMany()` | User → Posts |
| Appartient à | `belongsTo()` | Post → User |
| Plusieurs-à-plusieurs | `belongsToMany()` | User ↔ Role |
| Un-à-un | `hasOne()` | User → Profile |

---

## ⚠️ Pièges Courants

- ❌ Oublier `$fillable` → Insertion en masse impossible
- ❌ Mauvaise relation → Données inaccessibles
- ❌ Pas de cast pour JSON → Retour string au lieu d'array
- ❌ Convention de noms cassée → Relation échouée

---

## ✅ Validation Finale

- [ ] Modèle créé dans `app/Models/`
- [ ] `$fillable` défini
- [ ] `$casts` configurés si besoin
- [ ] Relations testées
- [ ] Aucune logique métier dans le modèle
- [ ] Tests `create()`, `update()` fonctionnels
