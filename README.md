# TP4 : Docker et Docker Compose (MERN)

## Objectif
Containeriser une application MERN (MongoDB, Express, React, Node.js) avec Docker, puis l'orchestrer avec Docker Compose.

## Etapes realisees

### 1. Cloner le projet
git clone https://github.com/wiem-kb/MyAppReact.git

### 2. Dockerfiles
- server/Dockerfile : image node:lts-alpine, port 9000, demarrage avec npm start
- client/Dockerfile : image node:lts-alpine, build Vite (npm run build), serveur serve, port 3000

Construction des images :
docker build -t mern-server ./server
docker build -t mern-client ./client

### 3. Reseau Docker
docker network create mern-network

### 4. MongoDB
docker run -d --name mongodb --network mern-network mongo

### 5. Serveur et client
docker run -d --name server --network mern-network -p 9000:9000 mern-server
docker run -d --name client --network mern-network -p 3000:3000 mern-client

### 6. Docker Compose
Fichier docker-compose.yml avec les services mongodb, server et client, reliés par le réseau mern-network (driver bridge). La variable MONGO_URI est définie pour le serveur.

Lancement :
docker compose up --build

## Verification
- Client : http://localhost:3000
- API : http://localhost:9000/api/health
