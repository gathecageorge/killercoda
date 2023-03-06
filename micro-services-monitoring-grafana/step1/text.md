## Prometheus
![Prometheus Image](https://miro.medium.com/v2/resize:fit:720/format:webp/0*FqL35xvASbIQfYbd.png)

Prometheus is an open source tool that collects metrics and stores them for easier querying using PromQL language. You can collect metrics exposed by any supported source. Using this metrics you can easily setup alerts so that you can be notified when something interesting happens in your server environment like service down.

To collect metrics about any service, you will need to configure prometheus with scrape targets. This is usually the endpoint and port where metrics are: e.g. `myservice:9000`. In the sample config below, we have prometheus scraping itself at url `localhost:9090`.

You configure prometheus using a yaml file with configuration options. Create a file `nano prometheus.yml` with the contents below.

```yaml
global:
  scrape_interval: 5s
  evaluation_interval: 5s

scrape_configs:
  - job_name: prometheus
    scrape_interval: 5s
    static_configs:
      - targets: 
        - 'localhost:9090' # Prometheus scrape itself
        labels:
          instance: 'prometheus'

```

To paste commands, use `Ctrl + Shift + V`

From here we can start prometheus using the command below. The first command creates a docker network that will be used for prometheus.

```bash
docker network create prometheus-network

docker run -d --network prometheus-network --name prometheus -p 9090:9090 -v $PWD/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus:latest
```

## Explanation

1. `-d` Starts prometheus in detached/background mode
2. `--name` provides the name to the container
3. `-p` We map port that prometheus uses to the host machine
4. `-v` We mount the file we created above inside the container `/etc/prometheus/prometheus.yml` so prometheus can start with that configuration.
5. `prom/prometheus:latest` Specify the latest prometheus docker image

To access the prometheus port in the browser, In this interactive environment you need to open `Traffic / Ports` page by clicking on top right side as below.

Open image in new tab if not visible.
![Access Traffic / Ports Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/images/Access_Port.png)

Once you open the page, you can enter the port `9090` under `Custom Ports` and then click `Access`. A new tab will be opened going to the application as shown below.

Open image in new tab if not visible.
![Open Custom Ports Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/images/Open_Port.png)


When you access prometheus page, you can click on Status Dropdown and Choose Targets. Here you will see all endpoints that prometheus is scraping as shown below.

Open image in new tab if not visible.
![Prometheus Scrape Targets](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/prometheus1.png)


## Add Another scrape target - Node exporter
We can run node exporter container so that we can add a custom target for prometheus to scrape. Node exporter provides metrics about the server like memory usage, disk usage, network etc. To run node exporter, use command below.

```bash
docker run -d --network prometheus-network --name node-exporter -v /etc/machine-id:/etc/machine-id:ro -v /proc:/host/proc:ro -v /sys:/host/sys:ro -v /:/rootfs:ro prom/node-exporter:latest --path.procfs=/host/proc --path.rootfs=/rootfs --path.sysfs=/host/sys --collector.filesystem.ignored-mount-points="^/(sys|proc|dev|host|etc)($$|/)"
```

Once you have node exporter running, you need to edit prometheus so that you can add another scrape target. Instead of making a change to prometheus everytime you want to add another target, you can use auto discovery functionality of prometheus. Thus any `yml` file inside file_sd directory will be auto scraped.

Create a new directory called file_sd using command below

```bash
mkdir file_sd
```

After modify the `nano prometheus.yml` file to be like below.

```yaml
global:
  scrape_interval: 5s
  evaluation_interval: 5s

scrape_configs:
  - job_name: prometheus
    scrape_interval: 5s
    static_configs:
      - targets: 
        - 'localhost:9090' # Prometheus scrape itself
        labels:
          instance: 'prometheus'

    # Auto discover scrape endpoints
    file_sd_configs:
      - files:
          - "/etc/prometheus/file_sd/*.yml"

```

You need to now remove the prometheus container and start it again using commands below. We have added another mount point, linking the file_sd directory created above to one inside the container.

```bash
docker rm -f prometheus

docker run -d --network prometheus-network --name prometheus -p 9090:9090 -v $PWD/file_sd:/etc/prometheus/file_sd -v $PWD/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus:latest
```

To scrape node exporter now, just create a new file inside file_sd directory called `nano file_sd/node-exporter.yml` and following contents

```yaml
- labels:
    instance: node-exporter
  targets:
  - node-exporter:9100

```

We can also add another scrape endpoint for an `imaginary` service called `webserver:8080`. As this service does not exist, we expect to see its state as Down when we check prometheus targets. Create a file `nano file_sd/webserver.yml` and following contents

```yaml
- labels:
    instance: webserver
  targets:
  - webserver:8080

```

You can now check the targets from prometheus. You will see 3 targets, 2 with status up and 1 status down as shown below.

Open image in new tab if not visible.
![Prometheus Scrape Targets](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/prometheus2.png)

To query metrics, in prometheus UI, click on Graph, type a query such as `node_memory_Active_bytes{job="prometheus"}` and click execute. From there you can view data as Table or Graph to see. See image below

Open image in new tab if not visible.
![Prometheus Query](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/prometheus3.png)
