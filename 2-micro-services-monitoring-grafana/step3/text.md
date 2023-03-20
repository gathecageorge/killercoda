## Promtail/Loki
![Promtail/Loki Image](https://miro.medium.com/v2/resize:fit:720/format:webp/0*RaNmq79lnboPtITY.png)

Every service will have their own logs. One way to view the logs from a service running in docker would be to use docker logs command as below:

```bash
docker logs -f --tail 10 mywebserver
```

However this way you can only view the logs and cannot do anything interesting with them. Plus if you have multiple services it becomes a problem to check for each.

Promtail helps in getting logs from different sources like files, stdin or docker driver. This logs are then forwarded to loki for storage. To identify the logs, labels are used to indicate source, service name, log type ie info, debug, error among many others. You can configure the labels as required for your use case.

Create a file called `nano promtail.yml` and following contents.

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 9081
  log_level: warn

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://loki:3100/loki/api/v1/push

scrape_configs:
# Collect logs from files in the system
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log

  - targets:
      - localhost
    labels:
      job: apache
      __path__: /var/log/apache2/*log

# Collect logs from docker containers
- job_name: containers
  static_configs:
  - targets:
      - localhost
    labels:
      job: containerlogs
      __path__: /var/lib/docker/containers/*/*log

  pipeline_stages:
  - json:
      expressions:
        output: log
        stream: stream
        attrs:

  - json:
      expressions:
        tag:
      source: attrs

  - regex:
      expression: (?P<image_name>(?:[^|]*[^|])).(?P<container_name>(?:[^|]*[^|])).(?P<image_id>(?:[^|]*[^|])).(?P<container_id>(?:[^|]*[^|]))
      source: tag

  - timestamp:
      format: RFC3339Nano
      source: time

  - labels:
      stream:
      container_name:

  - labeldrop:
    - filename

  - output:
      source: output
```

Loki stores the logs collected by Promtail and also retrieves them when querying. For ease of use, you can use grafana frontend for querying both loki logs and Prometheus metrics.

Create a file `nano loki.yml` and following contents.

```yaml
auth_enabled: false

server:
  http_listen_port: 3100
  grpc_listen_port: 9096

common:
  path_prefix: /tmp/loki
  storage:
    filesystem:
      chunks_directory: /tmp/loki/chunks
      rules_directory: /tmp/loki/rules
  replication_factor: 1
  ring:
    instance_addr: 127.0.0.1
    kvstore:
      store: inmemory

schema_config:
  configs:
    - from: 2020-10-24
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h
```

We can then start both promtail and loki containers to get logs from our services using commands below.

```bash
docker run -d --network prometheus-network --name promtail -v /etc/machine-id:/etc/machine-id:ro -v /var/log:/var/log:ro -v /var/lib/docker/containers:/var/lib/docker/containers:ro -v $PWD/promtail.yml:/etc/promtail/promtail.yml grafana/promtail:latest --config.file=/etc/promtail/promtail.yml

docker run -d --network prometheus-network --name loki -v $PWD/loki.yml:/etc/loki/loki.yml grafana/loki:latest --config.file=/etc/loki/loki.yml
```

Once you have the container running, we can go back to the grafana UI from previous step and add Loki as data source. Choose Loki Data source and add URL as below.

`http://loki:3100`

Open image in new tab if not visible.
![Loki Datasource Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/2-micro-services-monitoring-grafana/images/loki1.png)

In order to view logs, we can create a container that will create log messages. We use random logger docker image. This will run a random log generator generating logs randomly one for each time between 100 and 400 milliseconds. Run this command to start a service that will have logs. 

`NB The --log-driver and --log-opt are required so that promtail can pick up the logs for the container. That is why it will not pick up logs from the other containers like grafana or prometheus that dont have this options.`

```bash
docker run -d --name randomlogger --log-driver json-file --log-opt tag="{{.ImageName}}|{{.Name}}|{{.ImageFullID}}|{{.FullID}}" chentex/random-logger:latest 100 400
```

To view or search the logs in grafana, click on explore Icon on leftmost side and choose Loki data source that you created above. You can choose `code` option for query and write a query for the container you need to view logs for. 

e.g `{container_name="randomlogger", job="containerlogs"}`.

If you need filter the logs by type eg ERROR, DEBUG, INFO, WARN etc you can use Loki query language to filter 

e.g `{container_name="randomlogger", job="containerlogs"} |= "ERROR"`

Open image in new tab if not visible.
![Loki Logs View Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/2-micro-services-monitoring-grafana/images/loki2.png)
