---
description: Workflow complet de conception, développement et validation d’une fonctionnalité applicative.
---

# Workflow : Développement d’une Fonctionnalité

Ce workflow décrit le processus standard à suivre pour implémenter une nouvelle fonctionnalité, depuis la base de données jusqu’à l’interface utilisateur, en respectant la séparation des responsabilités entre les skills.

---

## Trigger
Déclenché lorsqu’une nouvelle fonctionnalité est demandée  
(ex: CRUD, module métier, amélioration fonctionnelle).

---

## Étapes

### Étape 1 : Préparation des Données
- **Skill** : `agent-database`
- **Objectif** : Mettre en place la structure de données nécessaire.

**Actions** :
1. Créer ou adapter les migrations (tables, colonnes, relations).
2. Définir les modèles Eloquent et leurs relations.
3. Ajouter des seeders ou factories si des données de test sont nécessaires.

**Output** :
- Migrations validées
- Modèles fonctionnels
- Base de données prête à l’usage

⛔ **Règle** : Ne jamais modifier une migration déjà exécutée.

---

### Étape 2 : Implémentation de la Logique Métier
- **Skill** : `agent-service`
- **Objectif** : Centraliser les règles métier et les traitements complexes.

**Actions** :
1. Créer un service métier dédié à la fonctionnalité.
2. Implémenter les règles de gestion (create, update, delete, etc.).
3. Mettre en place les policies pour sécuriser l’accès aux actions.
4. Utiliser des transactions si plusieurs opérations sont liées.

**Output** :
- Classe Service fonctionnelle
- Règles métier isolées
- Autorisations définies

⛔ **Interdiction** : Aucune logique métier dans les contrôleurs.

---

### Étape 3 : Exposition via HTTP
- **Skill** : `agent-http`
- **Objectif** : Connecter la fonctionnalité à l’utilisateur via des routes sécurisées.

**Actions** :
1. Définir les routes nécessaires (web ou api).
2. Créer les FormRequests pour valider les données entrantes.
3. Implémenter le contrôleur en appelant uniquement le service métier.

**Output** :
- Routes accessibles
- Données validées
- Contrôleurs légers et lisibles

⛔ **Règle** : Toute entrée utilisateur doit être validée.

---

### Étape 4 : Construction de l’Interface Utilisateur
- **Skill** : `agent-frontend`
- **Objectif** : Fournir une interface claire, interactive et responsive.

**Actions** :
1. Créer les vues Blade nécessaires.
2. Réutiliser les composants UI existants (Preline UI).
3. Ajouter l’interactivité avec Alpine.js.
4. Intégrer les icônes et assurer le responsive design.

**Output** :
- Interface fonctionnelle
- UX fluide
- Code frontend léger

⛔ **Interdiction** : Utiliser des frameworks JS lourds.

---

## Checkpoint Final
- Tester la fonctionnalité complète (du formulaire à la base de données).
- Vérifier les autorisations selon les rôles.
- Valider la conformité avec les règles des skills.

