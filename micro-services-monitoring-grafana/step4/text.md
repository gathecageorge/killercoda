## Portainer

Portainer is a powerful tool that helps in container management. Using this tool, its easy to deploy new services using a web interface. You can read more about it here https://www.portainer.io/

To run portainer, use the command below
```bash
docker run -d -p 8000:8000 -p 9443:9443 -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce:latest
```

## Portainer User Interface
If you want to see portainer running, open it on port `9000` like shown on previous steps how to open a port. It will allow you to see the containers running at a glance and also run others from its web interface.

The first time you open the UI, you will have to set a username and password as required.

![Portainer Setup](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/portainer1.png)

After that just click on `Getting Started` from the `Environment Wizard`.

![Portainer Getting Started](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/portainer2.png)

From there you will see the local environment indicating how much resources you have and number of container running etc.

![Portainer Local environment](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/portainer3.png)

### Deploy container
You can Click on container, then add container. Fill details as below. This will deploy an nginx web server at port `8081` which you can open to see default nginx page.

![Portainer deploy container](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/portainer4.png)

![Portainer deploy container](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/portainer5.png)

### Deploy docker compose stack
Now lets deploy something more complicated. We will deploy a php application and mysql database server. The php application will connect to the database at startup. Click on Stack, Add new Stack, Repository and fill details as below.

Name: `mystack`

Repository URL: `https://github.com/gathecageorge/php-docker-template.git`

Compose Path: `docker-compose.yml`

You also need to add this environment variables. Click on advanced mode to be able to put the values in text form. See image below.

![Portainer deploy stack](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/portainer6.png)

Environment variables content
```yaml
COMPOSE_PATH_SEPARATOR=:
COMPOSE_FILE=docker-compose.yml
WEB_SERVER_PORT=8082
PHPMYADMIN_PORT=8083
DATABASE_ROOT_PASSWORD=root
DATABASE_USER=testinguser
DATABASE_USER_PASSWORD=AveryHardPassword
MYSQL_DATABASE=testingdatabase
MYSQL_HOST=mysql
```

After that, just click deploy and wait. You can access PHP application at port `8082` and MySQL phpMyAdmin at port `8083`. Username and password for database are indicated above in config file. ie username `root` password `root` or username `testinguser` and password `AveryHardPassword`.

This session has shown you how to deploy containers or complex applications using portainer. If you want to remove any services, you can do so easily using portainer UI also.

