# mobilite-bordeaux-data

Plateforme de données sur la mobilité à Bordeaux, réunissant les horaires de transports (TBM), les vélos en libre-service (Le Vélo TBM) et la météo.

Le projet sert à montrer des bases solides en SQL, en NoSQL et en ingestion de données : collecter des données publiques, les stocker brutes, les nettoyer, puis les organiser en modèle en étoile pour les analyser.

## Questions du projet

1. Quelles stations de vélos sont le plus souvent vides, et à quelle heure ?
2. La pluie ou le froid changent-ils l'usage des vélos, et de combien ?
3. Quelles lignes de transport ont le plus de passages par heure ?

Précisions :
- Le flux des vélos (GBFS) ne garde pas d'historique : un relevé des stations est enregistré toutes les quelques minutes, et l'analyse n'a de sens qu'après plusieurs jours de collecte.
- L'usage des vélos n'est pas observé directement. Il est estimé par la baisse du nombre de vélos entre deux relevés, ce qui est approximatif (les camions de régulation faussent un peu les chiffres). Les résultats montrent donc ce qui est « associé à » la météo, pas ce qui est « causé par » elle.
- Les horaires GTFS sont des passages prévus : ils ne changent pas avec la météo.

## Sources de données

- Le Vélo TBM (GBFS) : informations et statut des stations, utilisés pour les questions 1 et 2.
- Réseau TBM (GTFS) : horaires des lignes, utilisés pour la question 3.
- Météo par heure (pluie, température) : source à confirmer.

Le détail de chaque source (format, fréquence, licence) sera dans `docs/sources.md`.

## Architecture prévue

Les données suivent ce chemin :

1. Les fichiers bruts (par exemple GTFS) sont stockés dans RustFS.
2. Les réponses JSON des API sont stockées dans MongoDB.
3. Les données chargées arrivent dans Postgres, schéma `raw`.
4. Elles sont nettoyées et typées dans le schéma `staging`.
5. Elles sont organisées en modèle en étoile (faits : relevés de stations et passages prévus ; dimensions : temps, météo, station, ligne, arrêt).
6. Les requêtes d'analyse, dans `sql/`, répondent aux trois questions.

## État du projet

En cours. La pile Docker locale est prête. Le choix des sources, l'ingestion en Python, le modèle en étoile et les requêtes restent à faire.

## Services Docker

- **postgres** (PostgreSQL 16) : entrepôt de données, port `5432`, base `mobilite`
- **mongo** (MongoDB 7) : stockage des données brutes semi-structurées, port `27017`
- **rustfs** (RustFS) : stockage objet compatible S3, API sur le port `9000`, console sur le port `9001`

Les identifiants sont lus depuis le fichier `.env` (à créer à partir de `.env.example`) :

```bash
cp .env.example .env
```

## Démarrer et arrêter la pile

```bash
# Démarrer
docker compose up -d

# Arrêter
docker compose down
```
