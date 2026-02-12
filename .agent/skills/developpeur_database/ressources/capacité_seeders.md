# Capacité : Peupler la Base avec Seeders

## 🎯 Contexte
Les seeders remplissent la base de données avec des données existantes, généralement importées à partir de fichiers CSV. Cela permet de préparer des données initiales cohérentes et reproductibles pour le développement et la production.

---

## 📋 Stratégies de Peuplement

| Contexte | Approche | Fichier Source |
|----------|----------|---|
| Données initiales statiques | Seeder CSV | `database/data/[model].csv` |
| Données pré-existantes | Import direct | Fichier CSV ou JSON |
| Relation entre modèles | Seeder cascadé | Multiple `calls()` |
| Données de test locales | CSV fixture | CSV dans `database/data/` |

---

## 🗂️ Approche Seeder CSV

### Étape 1 : Organiser la Structure des Fichiers
```
database/
├── data/
│   ├── users.csv
│   ├── posts.csv
│   └── categories.csv
└── seeders/
    ├── DatabaseSeeder.php
    ├── UserSeeder.php
    └── PostSeeder.php
```

### Étape 2 : Préparer le Fichier CSV
**Fichier : `database/data/users.csv`**
```csv
name,email,phone,is_active
Jean Dupont,jean@example.com,0601020304,1
Marie Martin,marie@example.com,0605060708,1
Pierre Durand,pierre@example.com,0612345678,0
```

**Points importants :**
- Première ligne = entêtes (obligatoire)
- Séparation par virgule
- Valeurs booléennes: 0/1 ou true/false
- Pas de guillemets manquants
- Encodage UTF-8 sans BOM

### Étape 3 : Créer le Seeder
```bash
php artisan make:seeder UserSeeder
```

### Étape 4 : Implémenter la Lecture CSV
```php
<?php

namespace Database\Seeders;

use App\Models\User;
use Illuminate\Database\Seeder;

class UserSeeder extends Seeder
{
    public function run(): void
    {
        $path = database_path('data/users.csv');
        
        // Vérifier l'existence du fichier
        if (!file_exists($path)) {
            $this->command->warn("Fichier CSV non trouvé: {$path}");
            return;
        }
        
        // Ouvrir le fichier
        $handle = fopen($path, 'r');
        if ($handle === false) {
            throw new \Exception("Impossible d'ouvrir le fichier CSV: {$path}");
        }
        
        // Passer la première ligne (entêtes)
        $headers = fgetcsv($handle);
        if (!$headers) {
            fclose($handle);
            return;
        }
        
        // Lire et créer les enregistrements
        $count = 0;
        while ($row = fgetcsv($handle)) {
            // Vérifier que la ligne a le bon nombre de colonnes
            if (count($row) !== count($headers)) {
                $this->command->warn("Ligne ignorée: colonnes manquantes");
                continue;
            }
            
            // Associer entêtes et valeurs
            $data = array_combine($headers, $row);
            
            // Créer l'enregistrement
            User::create($data);
            $count++;
        }
        
        fclose($handle);
        
        // Retour informatif
        $this->command->info("✓ {$count} utilisateurs créés");
    }
}
```

### Étape 5 : Enregistrer dans le DatabaseSeeder Principal
```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run(): void
    {
        // Exécuter les seeders dans l'ordre logique
        $this->call([
            UserSeeder::class,
            PostSeeder::class,
            CategorySeeder::class,
        ]);
        
        $this->command->info('✓ Tous les seeders ont été exécutés');
    }
}
```

---

## 🔄 Gestion des Dépendances Entre Entités

### Cas : Posts avec Utilisateurs
**Fichier : `database/data/posts.csv`**
```csv
title,content,user_id
Article 1,Contenu article 1,1
Article 2,Contenu article 2,1
Article 3,Contenu article 3,2
```

**Seeder :**
```php
public function run(): void
{
    $path = database_path('data/posts.csv');
    if (!file_exists($path)) return;
    
    $handle = fopen($path, 'r');
    fgetcsv($handle); // Skip header
    
    while ($row = fgetcsv($handle)) {
        Post::create([
            'title' => $row[0],
            'content' => $row[1],
            'user_id' => (int)$row[2],
        ]);
    }
    
    fclose($handle);
    $this->command->info('✓ Posts créés');
}
```

**Ordre d'exécution dans DatabaseSeeder :**
```php
$this->call([
    UserSeeder::class,    // D'abord les utilisateurs
    PostSeeder::class,    // Ensuite les posts
]);
```

---

## ⚠️ Bonnes Pratiques Seeders

### Validation des Données
```php
// ❌ MAUVAIS : créer sans vérifier
User::create($data);

// ✅ BON : utiliser le model avec validation
$user = new User();
$user->fill($data);
if (!$user->validate()) {
    $this->command->error("Données invalides: {$user->getErrors()}");
    continue;
}
$user->save();
```

### Gestion des Erreurs Robuste
```php
try {
    User::create($data);
} catch (\Exception $e) {
    $this->command->error("Erreur ligne {$count}: " . $e->getMessage());
    continue;
}
```

### Logging des Opérations
```php
// Avant insertion
$this->command->line("Création: {$data['name']}");

// Après insertion
$this->command->info("✓ {$count} utilisateurs ajoutés");

// En cas d'erreur
$this->command->warn("⚠ {$skipped} lignes ignorées");
```

---

## 🧪 Exécution des Seeders

### Exécuter Tous les Seeders
```bash
php artisan db:seed
```

### Exécuter un Seeder Spécifique
```bash
php artisan db:seed --class=UserSeeder
```

### Réinitialiser la Base et Seeder
```bash
# Supprimer toutes les données et repeupler
php artisan migrate:fresh --seed
```

### Afficher les Logs
```bash
php artisan migrate:fresh --seed --verbose
```

---

## ✅ Validation Finale

Checklist avant de déployer :

- [ ] Fichiers CSV présents dans `database/data/`
- [ ] Entêtes du CSV correspondent aux colonnes du model
- [ ] Tous les seeders appelés dans le bon ordre (dépendances)
- [ ] Gestion des erreurs pour fichiers manquants
- [ ] `php artisan db:seed` exécuté sans erreurs
- [ ] Données visibles en base avec `tinker` : `User::all()`
- [ ] Pas de doublons lors de multiples exécutions
- [ ] Encodage UTF-8 du CSV
