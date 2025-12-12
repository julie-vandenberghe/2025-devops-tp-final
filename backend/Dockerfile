# ---------- Étape 1 : compilation ----------
FROM golang:1.23-alpine AS builder

# Crée un dossier de travail
WORKDIR /app

# Copier tout le code source
COPY . .

# Compiler
RUN go build -o server ./cmd/server/main.go

# ---------- Étape 2 : execution ----------
FROM scratch

# Copier uniquement le binaire depuis l’image builder
COPY --from=builder /app/server .

# Exécuter avec un UID et GID non-root
USER 1000:1000

# Exposer le port 8080
EXPOSE 8080

# Démarrer l’application
ENTRYPOINT ["/server"]