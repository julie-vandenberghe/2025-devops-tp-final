# ---------- Étape 1 : compilation ----------
FROM node:20-alpine AS builder

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
FROM nginx:alpine AS runner

ENV BACKEND_URL=http://backend:8080/api/

WORKDIR /app

# Copie uniquement le build
COPY --from=builder /app/dist ./dist

# On copie la conf du site web
COPY ./localhost.conf /etc/nginx/conf.d/default.conf

RUN sed -i "s#BACKEND_URL#${BACKEND_URL}#g" /etc/nginx/conf.d/default.conf

# Passer à l'utilisateur non-root
#Since 1.17.0, both alpine- and debian-based images variants use the same user and group ids to drop the privileges for worker processes

# Exposer le port 80
EXPOSE 80