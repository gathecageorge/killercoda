## Create docker-compose.yaml file

We need to create a compose file specifying all the services we will need. Click on the editor Pane as shown below. Then right click on the explorer side to create a new file as shown. For file name enter `docker-compose.yml`

![Access editor](https://raw.githubusercontent.com/gathecageorge/killercoda/main/images/Editor.png)

Then put the contents below to the file.
```yaml
version: '3.1'

services:
  mysql-server:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password

  phpmyadmin:
    image: phpmyadmin:latest
    restart: always
    # We use port 8081 here because port 8080 is already 
    # been used by phpmyadmin in the previous step
    ports:
      - 8081:80
    # Instead of linking, we use environment variable to 
    # set the database server for phpmyadmin
    environment:
      - PMA_HOST=mysql-server


```

After that, you can just use the terminal within the editor to execute more commands as shown above or go back to `Tab 1`

Check that the file is saved by running command below. This will show the file content in the terminal.

```bash
cat docker-compose.yml
```

### Start the services

To start all the services specified in the file, run the command below. This starts the services in the background because of the `-d` attribute. You can now open the target ports and access the port as the previous step showed.

```bash
docker-compose -f docker-compose.yml up -d
```

<hr>

### Access New PhpMyAdmin
* [Click to open PhpMyAdmin "`http://localhost:8081` if on your machine"]({{TRAFFIC_HOST1_8081}})

<hr>

### Delete the services

To delete the services, just run command below. The `-v` means delete any docker volumes that might have been created by the services also. Volumes are like persistent storage for the containers.

```bash
docker-compose -f docker-compose.yml down -v
```

### Delete containers from previous step

This deletes the containers from previous step in one command. The `-f` means force remove the containers even if they are running. Otherwise you would have to stop `docker stop mysql-server phpmyadmin` them first and then remove them `docker rm mysql-server phpmyadmin`.

```bash
docker rm -f mysql-server phpmyadmin
```

