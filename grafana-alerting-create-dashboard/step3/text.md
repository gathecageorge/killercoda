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

Under details for yout alert rule, add annotation and choose summary. Add this as value `{{ $labels.instance}} is reporting {{ $values.B.Value }} Errors for the last 1 minutes.`

![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs2.png)

Click on `save and exit`.

Click save when back on dashboard and then go back. You should receive an alert after a while as below.
![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs3.png)


## Whats next
From here you can create other alerts as needed.
When an alert condition is resolved, say when memory is no longer above 70% or logs are below 5, then you will get an alert of type `RESOLVED` as below

Resolved memory usage
![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs4.png)

Resolved logs (Note have changed the values trigger resolved like adding high number of logs to alert. Same for memory.)
![Logs](https://raw.githubusercontent.com/gathecageorge/killercoda/main/grafana-alerting-create-dashboard/images/logs5.png)
