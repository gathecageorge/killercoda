## What is docker

Docker allows you to run your services inside containers. A container is a self contained package of your application and all dependencies required making it possible to run in any machine without any issues.

You can either package your application to a docker image or use some of the community images from docker hub or any image repository. 

In the first part of this tutorial, we will use the community docker image repository to deploy [mysql database server](https://hub.docker.com/_/mysql) and [phpmyadmin web interface](https://hub.docker.com/_/phpmyadmin) to access the database.

## docker-compose
Docker compose is a tool that helps to define and share multi container applications. Instead of deploying every container manually, you create a `yaml` file and then use docker compose to deploy all the services defined within that file.

In the second part of the tutorial, we will write a yaml file to deploy both the services above using one command instead of executing each container individually.

Lets jump straight to work and run some containers.
