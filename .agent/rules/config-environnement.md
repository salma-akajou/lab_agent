---
trigger: always_on
type: rule
id: config-environnement
---

# Configuration et Environnement

## Gestion de l’Environnement Laravel

- **Respect du fichier .env** : L'agent DOIT considérer que toute configuration sensible (DB, APP_KEY, MAIL, CACHE, etc.) provient exclusivement du fichier `.env` et ne doit jamais être codée en dur dans l’application.

- **Séparation Environnement / Code** : Aucune valeur spécifique à un environnement (local, staging, production) ne doit être intégrée directement dans les Controllers, Services ou Models.

- **Protection des données sensibles** : L'agent NE DOIT PAS afficher ni demander le contenu complet du fichier `.env`. Les exemples fournis doivent toujours utiliser des valeurs fictives.

- **Respect du .gitignore** : L'agent DOIT supposer que les dossiers comme `vendor/`, `node_modules/`, `storage/`, ainsi que le fichier `.env`, sont exclus du versioning et ne doivent pas être manipulés sans justification claire.

- **Configuration standard Laravel** : Toute recommandation doit suivre les conventions Laravel (config/*.php, variables d’environnement, cache config, etc.).

- **Stabilité avant optimisation** : Toute modification liée à l’environnement (cache, config, queue, session) doit privilégier la stabilité et la maintenabilité.
