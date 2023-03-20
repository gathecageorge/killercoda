## Setting up the tools using a github sample library.

We have learnt how to setup all these tools individually. In this part, we will use our portainer instance to setup everything from promtail, grafana, loki, promtail and prometheus.

For this we will use a github repo https://github.com/gathecageorge/vps-monitoring-prometheus-loki

This repo sets up everything without doing it one at a time. Before doing this, we need to remove all of the services we have created so far.

Use the commands below to remove all resources and clone the git repo.
```bash
# Remove all containers
docker rm -f $(docker ps -aq)

# Remove config files
rm -r loki.yml prometheus.yml promtail.yml file_sd/

# clone the repo
git clone https://github.com/gathecageorge/vps-monitoring-prometheus-loki.git

# Go into the cloned directory and list contents
cd vps-monitoring-prometheus-loki/ && ls -a

# Copy .env_template to .env (This contains configuration for grafana.)
cp .env_template .env 
```

You can open `.env` file to check the contents which are as below. They can be used if grafana is supposed to be accessed using a domain and reverse proxy like nginx or traefik.
```bash
GRAFANA_DOMAIN=localhost
GRAFANA_ADMIN_USER=myadmin
GRAFANA_ADMIN_PASSWORD=mypass
```

Once everything is done, just start all the services as below. NB We need to update `docker-compose`. By default version `1.25.0` is installed. We need latest version `(v2.5.0)`
```bash
# Update docker compose
curl -SL https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64 -o /usr/bin/docker-compose

# Start services
docker-compose up -d
```

# Congratulations
 You now have all the services up and running as before. 
 1. Open port [9000 portainer "`http://localhost:9000` if on your machine"]({{TRAFFIC_HOST1_9000}}) for portainer and setup password as before.
 2. Open port [3000 portainer "`http://localhost:3000` if on your machine"]({{TRAFFIC_HOST1_3000}}) and access grafana. 
 3. NB: 2 dashboards are imported automatically. One showing node-exporter metrics and another showing prometheus metrics.
 4. You dont have to add the data sources, they are added automatically. Both prometheus and loki data source.

Examples screenshots

![Loki logs expolore](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/end1.png)

![Prometheus explore](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/end2.png)

## Whats Next
Now you have learnt how to setup the tools for monitoring the services. From here we learn more about setting alerts, intergrating with slack, CI/CD to auto deploy services etc. Check back for more scenarios on this from my account https://killercoda.com/gathecageorge and more articles on medium https://gatheca-george.medium.com/

I will really appreciate if you took time to checkout medium for more information about this tools. Also feel free to comment with questions or suggestions and will be happy to reply.

👏 Happy learning. Cheers !!!

