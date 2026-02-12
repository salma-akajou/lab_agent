# Capacité : Créer/Modifier Schéma (Migration)

## 🎯 Contexte
Les migrations Laravel permettent de versionner et de contrôler précisément les modifications du schéma de base de données. Elles doivent être traitées comme du code source immuable une fois exécutées.

---

## 📋 Checklist Préalable
- [ ] La structure souhaitée est bien documentée
- [ ] Aucune migration similaire n'existe
- [ ] Les relations entre tables identifiées
- [ ] Les contraintes de sécurité définies

---

## 🔧 Étapes Pratiques

### Étape 1 : Analyser le Besoin
Avant de créer une migration, poser les questions :
- Quelle(s) table(s) affecter ?
- Ajouter, modifier ou supprimer des colonnes ?
- Quels types de données ? (string, integer, json, etc.)
- Des contraintes ? (unique, nullable, default)

### Étape 2 : Générer le Fichier
```bash
php artisan make:migration create_users_table
# ou
php artisan make:migration add_email_to_users_table
```

### Étape 3 : Implémenter up() et down()
- **up()** : L'action (créer table, ajouter colonne)
- **down()** : L'inverse (drop table, supprimer colonne)
- Toujours rendre le down() pleinement réversible

### Étape 4 : Valider
```bash
php artisan migrate  # Exécuter
php artisan migrate:rollback  # Tester le rollback
php artisan migrate  # Ré-exécuter
```

---

## ⚠️ Points d'Attention

### À Faire Absolument
✅ Utiliser les indices et contraintes (`foreign()`, `unique()`)  
✅ Ajouter `timestamps()` pour `created_at`/`updated_at`  
✅ Documenter les colonnes complexes (JSON, enum)  
✅ Tester à la fois `up()` et `down()`

### À Éviter Impérativement
❌ Modifier une migration déjà exécutée en production  
❌ Oublier `down()` (cause rollback impossibles)  
❌ Utiliser des noms génériques (`data`, `field`, `value`)  
❌ Créer plusieurs tables dans une seule migration

---

## 💡 Cas d'Usage Courants

**Créer une table** → `Schema::create()`  
**Ajouter une colonne** → `Schema::table()` + `$table->colonne()`  
**Modifier une colonne** → Change/modify sur colonne existante  
**Index/Unique** → `->unique()`, `->index()` directement sur colonne  
**Clé étrangère** → `$table->foreignId('user_id')->constrained()`

---

## ✅ Validation Finale

Avant committing:
- [ ] Migration exécutée sans erreur
- [ ] Rollback exécuté sans erreur
- [ ] Ré-migration possible
- [ ] Nommage clair et descriptif
- [ ] Pas de données hardcodées
