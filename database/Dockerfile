FROM postgres:14.1-alpine

#On copie les fichiers d'initialisation de la base de données
COPY sql/ docker-entrypoint-initdb.d

# On définie les informations de connexion à la bdd
ENV POSTGRES_DB=db \
   POSTGRES_USER=usr \
   POSTGRES_PASSWORD=pwd