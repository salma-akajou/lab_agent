---
name: agent-http
description: Gère les interactions HTTP entre l’utilisateur et l’application.
---

# Skill : Gestion des Requêtes HTTP

## 🎯 Rôle Principal
**Objectif** : Recevoir les requêtes utilisateurs, valider les données et déléguer le traitement à la couche métier.

---

## ⛔ Règles de Base
- ❌ Pas de logique métier dans les contrôleurs.
- ✅ Validation obligatoire via FormRequest.
- 🔐 Vérification des autorisations avant traitement.

---

## ⚡ Actions

### Action 1 : Mapper les Ressources avec Routes
**Contexte** : Exposer les fonctionnalités via HTTP  
**Capacité détaillée** : [ressources/capacité_routes.md](ressources/capacité_routes.md)

**Séquence d'actions :**
1. Identifier les ressources (User, Post, Product, etc.)
2. Lister les opérations CRUD nécessaires
3. Créer les routes avec noms explicites
4. Grouper par middleware (auth, admin, api)
5. Tester avec `php artisan route:list`
6. Valider Route Model Binding si utilisé

**Validation** : Routes nommées, RESTful et testables

---

### Action 2 : Sécuriser l'Entrée avec Validation
**Contexte** : Garantir la qualité et la sécurité des données reçues  
**Capacité détaillée** : [ressources/capacité_formrequest_validation.md](ressources/capacité_formrequest_validation.md)

**Phases d'exécution :**
1. Créer FormRequest pour chaque action (store, update)
2. Implémenter `authorize()` pour les droits d'accès
3. Définir les règles de validation dans `rules()`
4. Ajouter messages d'erreur localisés en français
5. Implémenter nettoyage (`prepareForValidation()`, `passedValidation()`)
6. Tester cas valide et cas d'erreur

**Validation** : Données validées et nettoyées avant traitement

---

### Action 3 : Orchestrer avec le Contrôleur
**Contexte** : Coordonner requêtes → services → réponses  
**Capacité détaillée** : [ressources/capacité_controleur.md](ressources/capacité_controleur.md)

**Phases d'exécution :**
1. Générer contrôleur avec `php artisan make:controller`
2. Injecter les services en constructor
3. Implémenter les 7 méthodes CRUD (index, create, store, show, edit, update, destroy)
4. Utiliser Route Model Binding où applicable
5. Vérifier autorisations avec `authorize()` ou policies
6. Retourner vues/redirects ou JSON selon le contexte

**Validation** : Contrôleur léger, logique métier en service

---

## 🔄 Exemple de Cycle HTTP
1. Définition de la route.
2. Validation via FormRequest.
3. Appel du service métier.
4. Retour d'une réponse (vue ou redirect).

---

## 📐 Bonnes Pratiques
- Contrôleurs fins.
- Méthodes ordonnées (CRUD).
- Sécurité avant fonctionnalité.
