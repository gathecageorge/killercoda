## Create alert from logs

We need to create an alert from logs.

1. Go to dashboard and choose the previous dashboard `My Dashboard`. 
2. Click on add panel icon and choose add new panel
![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs1.png)
3. For name add `Loki Logs`
4. For time choose 30min
5. For data source choose Loki
6. Choose code option and enter query `count_over_time({container_name="loki"} != "level=info" [1m])`. This counts the number of non info logs produced by loki every 1 min.
7. Expand options and lengend custom. Enter `{{container_name}}`
8. Click save and then go to alert.

## Add alert

Lets say you want to be alerted when there are more than 5 error logs every 1 min over a 5min period. Enter `5` for value C.

Choose Folder as `My alerts` and Evaluation group as `My alerts`. For time choose 1min so alert is evaluated faster instead of 5min

Under details for your alert rule, add annotation and choose summary. Add this as value `{{ $labels.container_name}} is reporting {{ $values.B.Value }} Errors for the last 1 minutes.`

![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs2.png)

Click on `save and exit`.

Click save when back on dashboard and then go back. You should receive an alert after a while as below.
![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs3.png)

## Task
To test your learning, create a new panel to show error/warn logs from a random logger docker container. You can start the container using command below. This will generate random logs every 1-3 seconds.

```bash
docker run -d --name randomlogger --log-driver json-file --log-opt tag="{{.ImageName}}|{{.Name}}|{{.ImageFullID}}|{{.FullID}}" chentex/random-logger:latest 1000 3000
```

Once the container is running, you can visualize the logs count per minute using Explore in grafana and the following query. (Select non debug/info logs count)
```bash
count_over_time({container_name="randomlogger"} !~ "DEBUG|INFO" [1m])
```

From the dashboard, create an alert so you can be notified when error the value falls below 1. This means be alerted when the container is stopped or does not exist since if its there then the value should always be greater than 1.

## Whats next
From here you can create other alerts as needed.
When an alert condition is resolved, say when memory is no longer above 70% or logs are below 5, then you will get an alert of type `RESOLVED` as below

Resolved memory usage
![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs4.png)

Resolved logs (Note have changed the values trigger resolved like adding high number of logs to alert. Same for memory.)
![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs5.png)
