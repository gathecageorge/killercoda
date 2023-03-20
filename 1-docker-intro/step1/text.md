## Run mysql database

We will use the official docker image for mysql database [mysql:latest](https://hub.docker.com/_/mysql) to run the database server. Copy or write the following command to deploy mysql database server. Its recommended to write the command to for better understanding and mastery.

To paste commands, use `Ctrl + Shift + V`

```bash
docker run --name mysql-server -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
```

### Explanation

1. `--name` Provides the container with a name in this case `mysql-server`
2. `-e` Sets an environment variable inside the container. In this case setting the root password for the database. Here you can provide any number of environment variables required by your application by specifying many `-e` variables. E.g `-e ENV_VAR_1=value1 -e ENV_VAR_2=value2 -e ENV_VAR_3=value3`
3. `-d` means we run container in detached mode, ie run in background

## Run phpmyadmin

In order to access the database, we will use [phpmyadmin:latest](https://hub.docker.com/_/phpmyadmin). This will be linked to the mysql-server and expose a web interface on port `8080`, from there you can login to the database using username `root` and password `password`

```bash
docker run --name phpmyadmin -d --link mysql-server:db -p 8080:80 phpmyadmin:latest
```

### Explanation

1. `--link` This is used to link the mysql database to the phpmyadmin. This means the containers can communicate with each other
2. `-p` This allows us to expose a port from the container so that we can access it outside in host machine. We want to access port `80` inside container using port `8080` outside in the host. In your machine for example, opening [http://localhost:8080](http://localhost:8080) will open the container port `80`. 

<hr>

## Connect to database

In this interactive environment, you can use the link below to access:
* [Click to open PhpMyAdmin "`http://localhost:8080` if on your machine"]({{TRAFFIC_HOST1_8080}})

If you need to open any other port, click on `Traffic / Ports` page by clicking on top right side as below.
![Access Traffic / Ports Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/images/Access_Port.png)

Once you open the page, you can enter the port under `Custom Ports` and then click `Access`. A new tab will be opened going to the application as shown below.
![Open Custom Ports Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/images/Open_Port.png)

<hr>

## Conclusion

Congratulations, you have successfully run 2 docker containers and accessed the web interface exposed by one of the services. In the next step, we learn how to use docker compose to run the same services using one command.

## Bonus

You can confirm all containers using the command below. The `-a` allows you to view all containers whether running or not. If you just want running ones, omit the -a.

```bash
docker ps -a
```
