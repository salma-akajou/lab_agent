# Rule : Architecture Laravel Claire

Cette règle définit la structure générale à respecter dans un projet Laravel
afin de garantir une bonne organisation du code.

## Objectif :
- Séparer clairement la logique métier de l’interface utilisateur.
- Faciliter la maintenance du projet à long terme.
- Permettre une meilleure compréhension du code par d’autres développeurs.

## Standards à respecter :
- Les Controllers doivent rester simples et lisibles.
- Toute la logique métier doit être placée dans des Services dédiés.
- Les vues Blade ne doivent contenir aucun traitement complexe.
- Les vues servent uniquement à afficher les données reçues.

## Résultat attendu :
Un projet structuré, cohérent et facile à faire évoluer.
