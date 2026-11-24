# TP4 : Docker et Docker Compose (MERN)

**Étudiant :** [gana iteb] | **Groupe :** [G3]
**Matière :** Introduction au DevOps | ISG de Sousse

## Structure du projet

- `Client/` : application React (Vite)
- `Server/` : API Express (Node.js)
- `docker-compose.yml` : orchestration des 3 services

## Étapes réalisées

### 1. Cloner le projet
```bash
git clone https://github.com/wiem-kb/MyAppReact.git
cd MyAppReact
```

### 2. Dockerfiles
- `Server/Dockerfile` : image `node:lts-alpine`, `npm install`, port 9000, `npm start`.
- `Client/Dockerfile` : image `node:lts-alpine`, `npm run build` (Vite, dossier `dist`), installation de `serve`, port 3000.
- Ajout de `.dockerignore` (exclut `node_modules`).

```bash
docker build -t mern-server ./Server
docker build -t mern-client ./Client
```

### 3. Réseau Docker
```bash
docker network create mern-network
```

### 4. MongoDB
```bash
docker run -d --name mongodb --network mern-network mongo
```

### 5. Serveur et client
```bash
docker run -d --name server --network mern-network -p 9000:9000 mern-server
docker run -d --name client --network mern-network -p 3000:3000 mern-client
```

### 6. Docker Compose
Le fichier `docker-compose.yml` définit les services `mongodb`, `server` (avec `MONGO_URI=mongodb://mongodb:27017/mydb`) et `client`, ainsi que le réseau `mern-network` (driver bridge).

```bash
docker compose up --build
```

## Vérification
- Client : http://localhost:3000
- Serveur : http://localhost:9000 (`Cannot GET /`, car l'API n'a pas de route `/`)
- État des conteneurs : `docker compose ps`

## Arrêter l'application
```bash
docker compose down
```