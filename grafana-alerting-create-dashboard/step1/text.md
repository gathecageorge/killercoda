## Setup test environment

We will use github repo https://github.com/gathecageorge/vps-monitoring-prometheus-loki to setup grafana/promtail/loki/prometheus/portainer as we did in the other scenario. 

The steps are outlined here but if you need more information you can open this scenario https://killercoda.com/gathecageorge/scenario/micro-services-monitoring-grafana

1. Clone the repo `git clone https://github.com/gathecageorge/vps-monitoring-prometheus-loki.git`
2. Change directory `cd vps-monitoring-prometheus-loki/`
3. Cp .env_template to .env `cp .env_template .env`
4. You can open `.env` and check its contents. Not required to change for test environment
5. Update docker-compose `curl -SL https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64 -o /usr/bin/docker-compose`
6. Run all services `docker-compose up -d`
7. Open grafana on port `3000`. See instructions to open port below.

In this interactive environment you need to open `Traffic / Ports` page by clicking on top right side as below.
![Access Traffic / Ports Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/images/Access_Port.png)

Once you open the page, you can enter the port under `Custom Ports` and then click `Access`. A new tab will be opened going to the application as shown below. Enter port number and click Access.
![Open Custom Ports Image](https://raw.githubusercontent.com/gathecageorge/killercoda/main/images/Open_Port.png)

Login to  grafana username `myadmin` and password `mypass`.


