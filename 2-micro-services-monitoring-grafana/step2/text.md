## Grafana
![Grafana Image](https://miro.medium.com/v2/resize:fit:720/format:webp/0*DkAgeD1S9-_VWtz1.png)

“Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources.” https://en.wikipedia.org/wiki/Grafana

Using grafana, you can query for the logs or metrics by adding Prometheus and Loki as a data source. You can also setup alerts and add dashboards as seen in image above.


## Start grafana
To run grafana use command below. This will launch grafana and map port 3000 to it. You can then open port `3000` as shown in previous step or link below and login using username `myadmin` and password `mypass`.

```bash
docker run -d --network prometheus-network --name grafana -p 3000:3000 -e GF_USERS_ALLOW_SIGN_UP=false -e GF_SECURITY_ADMIN_USER=myadmin -e GF_SECURITY_ADMIN_PASSWORD=mypass grafana/grafana:latest
```

* [Click to open Grafana "`http://localhost:3000` if on your machine"]({{TRAFFIC_HOST1_3000}})

You need to click on Settings Icon and go to Data Sources as shown below.

Open image in new tab if not visible.
![Grafana Data Source](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/grafana1.png)

Choose prometheus data source type and enter details as shown. After that click `Save & test`.
1. `URL` http://prometheus:9090

Open image in new tab if not visible.
![Grafana Data Source](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/grafana2.png)

You can then go to Dashboards and click import. Enter ID `1860` on the `Import via grafana.com` and click load. See below

Open image in new tab if not visible.
![Grafana Dashboard import](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/grafana3.png)

Choose data source as prometheus and click import.

Open image in new tab if not visible.
![Grafana Dashboard import](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/grafana4.png)

Once done, you will see a dashboard thats generated from prometheus metrics scraped from node-exporter. You can filter by time as required to see usage over time.

Open image in new tab if not visible.
![Grafana Dashboard](https://raw.githubusercontent.com/gathecageorge/killercoda/main/micro-services-monitoring-grafana/images/grafana5.png)
