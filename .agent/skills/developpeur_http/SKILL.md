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

## 🧭 Responsabilités

### 1️⃣ Gestion des Routes
- Définir des routes claires et nommées.
- Regrouper les routes par middleware.
- Séparer web et API.

### 2️⃣ Contrôleurs
- Méthodes simples et lisibles.
- Utilisation du Route Model Binding.
- Appel direct aux services métier.

### 3️⃣ Validation des Données
- Créer des FormRequests dédiées.
- Centraliser les règles de validation.
- Messages d’erreur clairs.

---

## 🔄 Exemple de Cycle HTTP
1. Définition de la route.
2. Validation via FormRequest.
3. Appel du service métier.
4. Retour d’une réponse (vue ou redirect).

---

## 📐 Bonnes Pratiques
- Contrôleurs fins.
- Méthodes ordonnées (CRUD).
- Sécurité avant fonctionnalité.
