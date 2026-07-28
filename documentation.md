# Documentation Technique

Ce document fournit un aperçu de l'architecture technique, du modèle de données et des concepts clés utilisés dans le projet Mini-LinkedIn (Plateforme RH).

## Architecture Globale

Le projet suit une architecture client-serveur classique :
- **Frontend** : Application Single Page (SPA) React gérant l'interface utilisateur, le routage (React Router) et les appels API (Axios).
- **Backend** : API REST Spring Boot gérant la logique métier, la sécurité et l'accès aux données.
- **Base de Données** : PostgreSQL pour le stockage persistant et relationnel des données.

## Modèle de Données (Entités Principales)

L'application repose sur plusieurs entités clés (qui peuvent être étendues selon les fonctionnalités) :

1. **User / Utilisateur**
   - Représente un compte sur la plateforme (Candidat, Employé, RH, ou Administrateur).
   - *Attributs types* : `id`, `email`, `password` (crypté), `role`, `firstName`, `lastName`, `profilePicture`.

2. **Profile / Profil Professionnel**
   - Détails professionnels d'un utilisateur.
   - *Attributs types* : `titre`, `description`, `competences`, `experiences`, `formations`.

3. **JobOffer / Offre d'Emploi**
   - Publiée par les recruteurs/RH.
   - *Attributs types* : `titre`, `description`, `entreprise`, `localisation`, `typeContrat`, `datePublication`.

4. **Application / Candidature**
   - Lien entre un Utilisateur et une Offre d'emploi.
   - *Attributs types* : `dateCandidature`, `statut` (En attente, Acceptée, Refusée), `cv`.

## Sécurité et Authentification

La sécurité est gérée par **Spring Security** et des **JSON Web Tokens (JWT)**.

- **Flux de Connexion** :
  1. L'utilisateur envoie ses identifiants (`email`, `password`) au point de terminaison `/api/auth/login`.
  2. Le backend vérifie les identifiants en base de données (le mot de passe est haché via `BCrypt`).
  3. Si valide, le backend génère un token JWT signé et le retourne au client.
  4. Le client stocke le token (ex: localStorage ou HttpOnly cookie) et l'envoie dans l'en-tête `Authorization: Bearer <token>` pour toutes les requêtes suivantes.

- **Gestion des Rôles** :
  - Les routes API sont protégées selon les rôles (ex: Seul un utilisateur avec le rôle `ROLE_HR` peut créer une offre d'emploi).

## API Endpoints (Aperçu)

*(Note: Ceci est un aperçu général, se référer à Swagger UI pour la documentation exhaustive de l'API)*

### Authentification (`/api/auth`)
- `POST /login` : Authentification et récupération du JWT.
- `POST /register` : Création d'un nouveau compte.

### Utilisateurs (`/api/users`)
- `GET /me` : Récupérer les informations de l'utilisateur connecté.
- `GET /{id}` : Consulter le profil public d'un utilisateur.
- `PUT /{id}` : Mettre à jour son profil.

### Offres d'Emploi (`/api/jobs`)
- `GET /` : Lister toutes les offres (avec filtres et pagination).
- `GET /{id}` : Détails d'une offre.
- `POST /` : Créer une nouvelle offre (Nécessite le rôle RH).
- `DELETE /{id}` : Supprimer une offre.

### Candidatures (`/api/applications`)
- `POST /` : Postuler à une offre.
- `GET /job/{jobId}` : Voir les candidatures pour une offre.
- `PUT /{id}/status` : Mettre à jour le statut d'une candidature.

## Bonnes Pratiques de Développement

1. **DTOs (Data Transfer Objects)** : Utilisation de DTOs (via MapStruct) pour éviter d'exposer directement les entités JPA dans les contrôleurs REST.
2. **Gestion des Exceptions** : Utilisation de `@ControllerAdvice` pour capturer et formater de manière uniforme les erreurs renvoyées au client (ex: 404 Not Found, 400 Bad Request).
3. **Variables d'Environnement** : Ne jamais versionner les informations sensibles (mots de passe BDD, clés secrètes JWT). Utiliser des variables d'environnement ou des profils Spring (`application-dev.properties`, etc.).
