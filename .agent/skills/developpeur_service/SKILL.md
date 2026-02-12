---
name: agent-service
description: Centralise la logique métier et les règles de gestion de l’application.
---

# Skill : Logique Métier & Services

## 🎯 Rôle Principal
**Objectif** : Isoler toute la logique métier dans des classes dédiées afin de garder une architecture propre et maintenable.

---

## ⛔ Contraintes Importantes
- 🚫 Aucun code HTTP dans les services.
- 🚫 Aucun accès direct à `Request` ou `Auth` sans injection.
- 🔁 Utiliser des transactions pour les opérations critiques.

---

## ⚡ Actions

### Action 1 : Construire la Logique Métier
**Contexte** : Encapsuler la logique domaine dans des services réutilisables  
**Capacité détaillée** : [ressources/capacité_service_metier.md](ressources/capacité_service_metier.md)

**Séquence d'actions :**
1. Identifier le domaine métier (User, Order, Invoice, etc.)
2. Créer le fichier `app/Services/[Domain]Service.php`
3. Implémenter les méthodes CRUD (create, update, delete, getAll)
4. Injecter les dépendances (modèles, autres services)
5. Typer les retours (Model, Collection, bool)
6. Tester chaque méthode indépendamment

**Validation** : Service fonctionnel avec logique isolée

---

### Action 2 : Formaliser les Règles Domaine
**Contexte** : Centraliser les validations métier complexes et les calculs  
**Capacité détaillée** : [ressources/capacité_rules_metier.md](ressources/capacité_rules_metier.md)

**Séquence d'actions :**
1. Identifier les règles métier du domaine
2. Créer des méthodes privées pour chaque règle
3. Nommer explicitement (`validateOrderEligibility()`, `calculatePrice()`, etc.)
4. Lancer exceptions claires quand règles violées
5. Utiliser transactions pour opérations liées
6. Tester tous les cas limites (nominal + erreurs)

**Validation** : Règles testées et exceptions documentées

---

### Action 3 : Sécuriser l'Accès aux Ressources
**Contexte** : Contrôler qui peut faire quoi sur quelle ressource  
**Capacité détaillée** : [ressources/capacité_securite.md](ressources/capacité_securite.md)

**Séquence d'actions :**
1. Créer la Policy avec `php artisan make:policy [Resource]Policy`
2. Implémenter les méthodes standards (create, update, delete, etc.)
3. Ajouter `before()` pour bypass admin automatique
4. Vérifier propriété et rôles dans chaque méthode
5. Enregistrer dans `AuthServiceProvider`
6. Utiliser `$this->authorize()` en contrôleur
7. Tester tous les rôles et cas (admin, owner, others)

**Validation** : Autorisations testées, non-autorisés retournent 403

---

## 🔄 Exemple de Mise en Œuvre
1. Création du service.
2. Ajout des méthodes métier.
3. Création de la policy.
4. Utilisation du service depuis le contrôleur.

---

## 📐 Bonnes Pratiques
- Injection de dépendances.
- Code lisible et testable.
- Pas de logique métier dans les contrôleurs.
