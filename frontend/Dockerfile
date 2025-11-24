# ---------- Étape 1 : compilation ----------
FROM node:18-alpine AS builder

# Créer un répertoire pour l'application
WORKDIR /app

# Copier les fichiers
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier tout le code
COPY . .

# Lancer le build
RUN npm run build

# ---------- Étape 2 : execution ----------
FROM node:18-alpine AS runner

WORKDIR /app

# Installer serve pour servir les fichiers statiques
RUN npm install -g serve

# Copie uniquement le build
COPY --from=builder /app/dist ./dist

# Passer à l'utilisateur non-root
USER node

# Exposer le port 5173
EXPOSE 5173

# Démarrer l'app
CMD ["serve", "-s", "dist", "-l", "5173"]