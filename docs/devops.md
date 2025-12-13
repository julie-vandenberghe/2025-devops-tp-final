# Stratégie devops
### Auteur : Julie Vandenberghe 

## Docker
- Dockerfile pour le frontend (build + Nginx)
- DOckerfile pour le backend (build de l’API Go)
- Build des images Docker et push vers Docker Hub
- Docker Compose pour lancer l'ensemble de l'application (frontend, backend, database) en local
- Docker Compose spécifique utilisé en CI pour les tests end-to-end (Playwright)

---

## Déploiement

### Environnement choisis
- Frontend et Backend : Render.com
- Base de données : Neon
- Storybook : GitHub Pages

### Liens vers les environnements
- Frontend : [https://two025-devops-frontend-main.onrender.com/](https://two025-devops-frontend-main.onrender.com/)
- Backend : [https://two025-devops-backend-main.onrender.com](https://two025-devops-backend-main.onrender.com)
- Documentation Storybook : [https://julie-vandenberghe.github.io/2025-devops-tp-final/docs](https://julie-vandenberghe.github.io/2025-devops-tp-final/docs)
- Docker image pour le frontend : [https://hub.docker.com/repository/docker/julievandenberghe/2025-devops-frontend/general](https://hub.docker.com/repository/docker/julievandenberghe/2025-devops-frontend/general)
- Docker image pour le backend : [https://hub.docker.com/repository/docker/julievandenberghe/2025-devops-backend/general](https://hub.docker.com/repository/docker/julievandenberghe/2025-devops-backend/general)

---

## GitHub Actions

### CI (déclenchée à chaque push)
#### Frontend
- Installation des dépendances
- Lint (ESLint)
- Tests unitaires
- Build de l’application
- Build et push de l’image Docker sur Docker Hub

#### Backend
- Installation des dépendances Go
- Lancement d’une base PostgreSQL via service GitHub Actions
- Tests unitaires Go

#### Documentation
- Build de Storybook
- Déploiement automatique sur GitHub Pages

### E2E tests (déclenchés à chaque push)

### Pipeline CD (déclenchée à chaque success de la CI)
#### Frontend
- Image Docker stockée sur Docker Hub
- Déploiement sur Render
- Nginx sert l’application et redirige les appels /api vers le backend

#### Backend
- Image Docker stockée sur Docker Hub
- Déploiement sur Render
- Connexion à la base PostgreSQL Neon via variable d’environnement

### Release (déclenchée lorsque le tag suit le format v*)
- Création d'une release avec des releases notes basés sur les commits

---

## Gestion des variables d’environnement

Les variables sensibles ne sont jamais commitées. Exemples :
- DATABASE_URL : connexion PostgreSQL (Neon)
- BACKEND_URL : URL du backend pour le frontend

Elles sont définies :
- dans GitHub Secrets (CI/CD),
- dans les services Render (production).

---

## Stratégie de collaboration et qualité

Branche main protégée :
- pull request obligatoire pour merger dans main 
- besoin d’approbation d’un autre utilisateur

Message de commits :
- utilisation de gitmoji
- utilisation des messages de commits dans les release notes

---

## Difficultés rencontrées

- Configuration du proxy : difficultés de communication entre le frontend et le backend en production.
- Tests E2E : Exigent un environnement complet (BDD, Frontend, Backend) avec des secrets gérés. Complexité d'intégration dans le pipeline CI sans exposer les secrets.

---

## Améliorations

- Séparation plus stricte des environnements (staging / prod)
- Finalisation des tests E2E
- Trouver solution alternative pour déploiement (mise en veille rapide de Render qui provoque erreur 502)