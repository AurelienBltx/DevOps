# **Raport Devops**

## Dokker


_Question 1-2) Why do we need a multistage build? And explain each step of this dockerfile._

A multi-stage build in a Dockerfile is used to optimize the resulting Docker image size by separating the build environment from the runtime environment.

_Question 1-5) Document your publication commands and published images in dockerhub._

To publish image we need to login to Docker Hub with the command "docker login". We add a tag 
to now what version of the image it is with "docker tag tp-devops-database abourliatoux7/tp-devops-database:1.0". Finally we push the image with "docker push abourliatoux7/tp-devops-database:1.0"

_Question 2-1) What are testcontainers ?_

They are Java librairies which allows us to use Docker containers within our tests.