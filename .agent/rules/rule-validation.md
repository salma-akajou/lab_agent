---
trigger: always_on
---

# Rule – Validation obligatoire des données

## Principe (Hard Rule)
- **VALIDATION OBLIGATOIRE :** Toute donnée envoyée par l’utilisateur doit être validée avant tout traitement.  
- **PAS DE CONTROLE DIRECT:** Aucun accès direct aux données depuis le contrôleur sans validation.  
- **REGLES CLAIRES ET CONTEXTUALISEES:** Les règles de validation doivent être explicites et adaptées au contexte.

## Application & Workflow
1. Utiliser systématiquement des `FormRequest` pour les formulaires.  
2. Définir les règles de validation dans le `FormRequest`.  
3. Injecter les données validées dans le contrôleur : `public function store(StoreFilmRequest $request) { ... }`.  
4. Tout accès aux données non validées est **interdit**.

## Objectifs
- Éviter les données incorrectes ou malveillantes.  
- Améliorer la sécurité de l’application.  
- Garantir un comportement fiable et prévisible.

## Interdictions (⛔)
- Toute fonctionnalité qui manipule des données utilisateur sans validation est considérée comme **invalide**.  
- Ne jamais écrire de logique métier directement dans les Blade sans passer par des objets validés.
