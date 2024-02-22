# TareaMod2
TareaMod2
#Version 1.0a
#Author: Daniel Morera
#Description: Simple web app with 2 URLs
#URL 1 can be found at localhost:8000
#URL 2 can be found at localhost:8000/api.html



#in order to start this application you should run the following 
#docker compose up --build -d
#this command should be run in the tareaMod1 directory when you unpack tareaMod1.tgz



Explanation of the docker-compose file:




services:

 Pulls an nginx image
 and forwards port 8000 to port 80
  web:
    image: nginx:latest
    ports:
    - "8000:80"

#Pulls a postgres image
#the restart statment makes the container restart automatically (if not stopped manually)
#the healthcheck determines the health of the container on start up and checkes if postgress is actually up 
#every 1 seconds with a maximum retry counter of 10
#the environment statement is related to variables that are used on building postgres itself 
#in this case user, password and DB name

  db:
    image: meraxes6/tarea-db:halo
    restart: always
    healthcheck:
      test: ["CMD-SHELL", "pg_isready"]
      interval    : 1s
      timeout: 5s
      retries: 10
    environment:
      POSTGRES_PASSWORD: Csdqd3m0
      POSTGRES_DB: tarea
      POSTGRES_USER: postgres
    ports:
    - "5432:5432"
  adminer:
    image: adminer
    restart: always
    ports:
    - "8080:8080"
    depends_on:
      - db
