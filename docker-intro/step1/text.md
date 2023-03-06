## Run mysql database

We will use the official docker image for mysql database [mysql:latest](https://hub.docker.com/_/mysql) to run the database server.

```bash
docker run --name mysql-server -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
```

## Run phpmyadmin

In order to access the database, we will use [phpmyadmin:latest](https://hub.docker.com/_/phpmyadmin). This will be linked to the mysql-server and expose a web interface on port `8080`, from there you can login to the database using username `root` and password `password`

```bash
docker run --name phpmyadmin -d --link mysql-server:db -p 8080:80 phpmyadmin:latest
```

## Connect to database

If you were running the commands in your machine, you would need to open `http://localhost:8080` to access phpmyadmin web interface. In this interactive environment you need to check more here....
