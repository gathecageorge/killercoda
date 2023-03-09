## Create dashboard in grafana and setup alerts

In my other tutorial, we learnt how to setup grafana, loki/promtail and prometheus. This tools help in understanding how the services are doing.

However its useless if we just reach there. We want to be alerted when something happens in the system. i.e

1. When there are many error logs from an application
2. When disk capacity is almost running out
3. When there is a spike in CPU or memory usage
4. etc

To do that we need alerts. For this scenario, we will setup alerts to slack from grafana.
