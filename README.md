---
marp: true
theme: default
_class: lead
paginate: true
backgroundColor: #ffffff
color: #5B2C6F
---

# LAB Agent

**Presentée par :** Salma Akajou  
**Encadré par :** Mr. ESSARRAJ Fouad  
**Projet :** Lab Agent  
**Année :** 2025 / 2026

---

## Plan de la Présentation

1. Le concept de l’Agent  
2. Les Rules  
3. Les Workflows  
4. Les Skills  
5. Exemple d’utilisation  
6. Structure du dépôt

---

## 1. Définition de l’Agent

> Un Agent est une entité intelligente capable d’exécuter des tâches
de manière structurée et cohérente.

- Il analyse une demande utilisateur.
- Il comprend le contexte du projet.
- Il applique des règles et des méthodes définies à l’avance.
- Il produit un résultat organisé et réutilisable.

---

## 2. Rôle des Rules

Les **Rules** représentent les règles générales du projet.

- Elles définissent les bonnes pratiques à respecter.
- Elles sont toujours prises en compte par l’agent.
- Elles garantissent une cohérence globale du code.
- Elles limitent les erreurs liées à une mauvaise organisation.

📌 Exemple :  
Architecture Laravel, conventions de nommage, ...

---

## 3. Rôle des Workflows

Les **Workflows** décrivent un enchaînement logique d’actions.

- Ils sont déclenchés par une commande précise.
- Ils guident l’agent étape par étape.
- Ils assurent un déroulement clair du travail.
- Ils évitent les oublis lors du développement.

📌 Exemple :  
Création d’un module, mise à jour d’une fonctionnalité, refactorisation.

---

## 4. Rôle des Skills

Les **Skills** représentent les domaines d’expertise de l’agent.

- Chaque skill est spécialisé dans un sujet précis.
- Il est utilisé uniquement lorsque le contexte le nécessite.
- Il permet d’améliorer la qualité des réponses de l’agent.
- Il rend l’agent plus intelligent et plus ciblé.

📌 Exemple :  
 validation des formulaires, traduction Laravel, spatie.

---

## 5. Exemple d’Utilisation

### Demande :
`/build-ui-feature Film`

### Comportement de l’Agent :

- Il applique les **Rules** pour respecter l’architecture.
- Il utilise la **Skill Blade** pour construire l’interface.
- Il suit le **Workflow UI Feature** pour organiser les étapes.
- Il produit un code clair, structuré et fonctionnel.

---

## 6. Structure du Dépôt
```bash
.agent/
├── rules/
│ ├── architecture-laravel.md
│ └── naming-convention.md
├── skills/
│ └── laravel-translation.md
└── workflows/
  └── refactor-feature-flow.md
```