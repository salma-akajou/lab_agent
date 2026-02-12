---
name: developpeur_database
description: Gère la structure de la base de données, les modèles Laravel et les données de test.
---

# Skill : Gestionnaire Base de Données

## 🎯 Rôle Principal
**Objectif** : Concevoir et maintenir une base de données fiable, cohérente et performante en utilisant les outils Laravel (Migrations, Eloquent, Seeders).

---

## ⛔ Règles Fondamentales
- ❌ Ne jamais modifier une migration déjà exécutée → toujours en créer une nouvelle.
- 🔐 Toujours sécuriser les modèles avec `$fillable`.
- 🏷️ Respecter les conventions Laravel (tables au pluriel, modèles au singulier).

---

## ⚡ Actions

### Action 1 : Créer/Modifier Schéma (Migration)
**Contexte** : Créer/modifier la structure de la base de données  
**Capacité détaillée** : [ressources/capacité_migration.md](ressources/capacité_migration.md)

**Étapes du processus :**
1. Générer la migration avec `php artisan make:migration`
2. Implémenter `up()` pour les changements
3. Implémenter `down()` pour la réversion
4. Exécuter avec `php artisan migrate`
5. Tester le rollback avec `php artisan migrate:rollback`

**Validation** : Migration reversible et testée sans erreurs

---

### Action 2 : Définir Modèle Eloquent
**Contexte** : Mapper une table en modèle Eloquent exploitable  
**Capacité détaillée** : [ressources/capacité_modele_eloquent.md](ressources/capacité_modele_eloquent.md)

**Étapes du processus :**
1. Créer le modèle avec `php artisan make:model User`
2. Configurer `$fillable` pour protéger la masse
3. Ajouter `$casts` pour les types spécialisés
4. Déclarer les relations (belongsTo, hasMany, etc.)
5. Tester les opérations CRUD (`create()`, `update()`, `delete()`)

**Validation** : Modèle fonctionnel avec relations testées

---

### Action 3 : Peupler la Base avec Seeders
**Contexte** : Alimenter la base avec données de test ou réelles  
**Capacité détaillée** : [ressources/capacité_seeders.md](ressources/capacité_seeders.md)

**Étapes du processus :**
1. Créer Seeder pour données réelles avec `php artisan make:seeder UserSeeder`
2. Importer depuis CSV ou base de données existante
3. Implémenter la lecture de données
4. Insérer en base de données
5. Exécuter avec `php artisan db:seed`

**Validation** : Données présentes et accessibles en base

---

## 🔄 Exemple de Flux de Travail
1. Création d'une migration pour une nouvelle table.
2. Création du modèle associé.
3. Génération de données fictives.
4. Test avec `php artisan migrate --seed`.

---

## 📐 Bonnes Pratiques
- Toujours inclure `timestamps()`.
- Favoriser la lisibilité avant l’optimisation.
- Tester chaque migration avant validation.
