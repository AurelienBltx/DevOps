FROM postgres:14.1-alpine

#Copy the files in the package sql to initialisate the database in the image
COPY sql/ docker-entrypoint-initdb.d
