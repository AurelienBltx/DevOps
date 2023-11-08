# **Report Devops**

## Dokker

To simplify the development of our application, we'll use Docker. We need for that a Dockerfile for each part of the application to define how to build the images. 

We'll need one for the backend, one for the database and one for the server. 

To fill the database we have to .sql file to run. We put these files in a sql directory and we'll run the sql commands with the Dockerfile of the database.
I putted the database secrets variables in a .env file which will be ignored when commited.

For the backend, we use an API (Spring Initializer) to generate a Springboot application with this config :
   - Project: Maven
   - Language: Java 17
   - Spring Boot: 2.7.5
   - Packaging: Jar
   - Dependencies: Spring Web 
 
We'll also need a network to allow the containers to exchange data. For that we execute the command : 
      `docker network create app-network`
 - Then we run each image with :
 `docker build -t abourliatoux7/tp-devops-database`
------

_**Question 1-2** Why do we need a multistage build? And explain each step of this dockerfile._

A multi-stage build in a Dockerfile is used to optimize the resulting Docker image size by separating the build environment from the runtime environment.

------

_**Question 1-3** Document docker-compose most important commands.

`docker compose up` is the most important command as it runs the docker-compose file.
`docker-compose down` is also important. It stops and deletes all the containers defined in the docker-compose file.

------

_**Question 1-5** Document your publication commands and published images in dockerhub._

To publish image we need to login to Docker Hub with the command `docker login`. We add a tag 
to now what version of the image it is with `docker tag tp-devops-database abourliatoux7/tp-devops-database:1.0`. Finally we push the image with `docker push abourliatoux7/tp-devops-database:1.0`.

------

## Github Actions

To test our application we'll build pipelines with the help of Github Actions.
We've add some dependencies in the pom.xml to use testcontainers.

------

_**Question 2-1** What are testcontainers ?_

They are Java librairies which allows us to use Docker containers within our tests.

------
To run the tests, we have to build the tests. For that we use `mvn clean verify`

Now we'll build our CI. We create a new file main.yml to define the architecture of the pipeline. To see the pipeline run, we have to push the code 
on Github and then go in the Actions tab on Github.

To have access to secrets variables of my Docker Hub account, we have to create secrets variables on Github.
Now we'll be able to use them in our code with `secrets.USER_DOCKER`

See the main.yml file for more informations about run of jobs for the pipeline.

To know the quality and security of our code we'll use SonarCloud. We have to add SonarCloud in our job in main.yml to use it : 
`mvn -B verify sonar:sonar -Dsonar.projectKey=devops-2023 -Dsonar.organization=devops-school -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${{ secrets.SONAR_TOKEN }}  --file ./simple-api/pom.xml`

-------

## Ansible

In this last part of the TP we'll use Ansible to help us deploy our application on the server.

To use Ansible we built an inventory file where we define the server of deployment and the location of the ssh private key. We test the inventory by pinging with : `ansible all -i inventories/setup.yml -m ping`

We have to request the server to get the OS distribution with : `ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"`

We define the actions to realise with Ansible in a file playbook.yml. In this file, we'll divide the tasks in 5 roles : backend, database, docker, httpd, and network.
To create a role : `ansible-galaxy init roles/docker`

Finally we have all the roles defined so we can run the playbook.yml file to deploy the applicaion on aurelien.bourliatoux.takima.cloud. To execute the playbook we use `ansible-playbook -i inventories/setup.yml playbook.yml`
