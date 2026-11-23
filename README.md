# mobilite-bordeaux-data

Plateforme de données sur la mobilité à Bordeaux, réunissant les horaires de transports, le temps réel lorsqu'il est disponible et la météo.

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
