# Guide d'Exécution

Ce document décrit les étapes nécessaires pour configurer et exécuter le projet Mini-LinkedIn localement. Le projet est divisé en deux parties : un backend (Spring Boot) et un frontend (React/Vite).

## Prérequis

Avant de commencer, assurez-vous d'avoir installé les outils suivants sur votre machine :
- **Java 17** ou supérieur
- **Node.js** (version 18 ou supérieure recommandée) et **npm**
- **PostgreSQL** (version 13 ou supérieure)
- **Maven** (optionnel, le projet inclut le wrapper `mvnw`)

## 1. Configuration de la Base de Données

1. Ouvrez votre terminal PostgreSQL ou pgAdmin.
2. Créez une nouvelle base de données pour le projet :
   ```sql
   CREATE DATABASE linkfinal_db;
   ```
3. Assurez-vous que l'utilisateur et le mot de passe correspondent à ce qui est défini dans le fichier de configuration du backend (`backend/src/main/resources/application.properties` ou `application.yml`).
   Exemple de configuration standard :
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/linkfinal_db
   spring.datasource.username=postgres
   spring.datasource.password=votre_mot_de_passe
   ```

## 2. Exécution du Backend (Spring Boot)

Le backend utilise Maven. Vous pouvez utiliser le wrapper Maven inclus.

1. Ouvrez un terminal.
2. Naviguez vers le dossier du backend :
   ```bash
   cd backend
   ```
3. Compilez et exécutez le projet :
   - Sous **Windows** :
     ```cmd
     mvnw.cmd spring-boot:run
     ```
   - Sous **Mac/Linux** :
     ```bash
     ./mvnw spring-boot:run
     ```
4. Le backend démarrera et sera accessible par défaut sur `http://localhost:8080`.
5. La documentation de l'API (Swagger) devrait être disponible sur `http://localhost:8080/swagger-ui.html` (selon la configuration).

## 3. Exécution du Frontend (React + Vite)

Le frontend utilise npm pour la gestion des dépendances.

1. Ouvrez un nouveau terminal.
2. Naviguez vers le dossier du frontend :
   ```bash
   cd frontend
   ```
3. Installez les dépendances :
   ```bash
   npm install
   ```
4. Démarrez le serveur de développement :
   ```bash
   npm run dev
   ```
5. Le frontend sera accessible sur l'URL indiquée dans le terminal (généralement `http://localhost:5173`).

## Résolution des Problèmes Courants

- **Erreur de connexion à la base de données** : Vérifiez que PostgreSQL est en cours d'exécution et que les identifiants dans `application.properties` sont corrects.
- **Port déjà utilisé** : Si le port 8080 ou 5173 est déjà occupé, vous pouvez modifier les ports dans `application.properties` (pour Spring Boot) et `vite.config.js` (pour Vite).
- **Problème de CORS** : Si le frontend ne peut pas communiquer avec le backend, assurez-vous que la configuration CORS du backend autorise l'URL du frontend (`http://localhost:5173`).
