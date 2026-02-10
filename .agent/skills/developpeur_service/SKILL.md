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

## 🧠 Missions du Service

### 1️⃣ Création de Services Métier
- Un service = un domaine métier.
- Méthodes claires et bien nommées.
- Typage strict des paramètres et retours.

### 2️⃣ Implémentation des Règles Métier
- Appliquer les règles fonctionnelles.
- Gérer les exceptions métier.
- Retourner des objets cohérents (Model ou DTO).

### 3️⃣ Sécurité & Autorisations
- Définir des Policies par modèle.
- Vérifier les droits avant toute action sensible.
- Centraliser les règles d’accès.

### 4️⃣ Gestion Avancée
- Intégration des médias (Spatie MediaLibrary).
- Gestion des rôles et permissions (Spatie Permission).

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
