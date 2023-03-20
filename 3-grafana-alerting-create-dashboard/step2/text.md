## Add custom dashboard

To add custom dashboard, click on New Dashboard as shown below. Then `Add a new panel`

![Create dashboard](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard1.png)

You need to modify the panel as follows
![Create dashboard](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard2_extra.png)

1. For time choose `Last 30 minutes`
2. For title enter `Memory Usage`
3. For Data source choose `Prometheus`. Change to code from builder.
4. For Query type the following `100 - ((node_memory_MemAvailable_bytes{} * 100) / node_memory_MemTotal_bytes{})`. This calculates the percentage of the memory available in the server by taking the total free memory / total available memory * 100.
5. Expand the options and under `Lengend` choose custom option. Then enter `{{instance}}` as the value
6. Click on Save, enter the name of the dashboard to anything like `My Dashboard`

## Now to create alert. 
We want to be notified when memory usage exceed 70% for a 5min time period.

Edit the panel as shown below.

![Edit panel](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard2.png)

Click on `Create alert rule from this panel`

![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard3.png)

When the alerts page is open edit the value in C to the percentage you want to be alerted when reached. In this case alert if the memory usage is above 70%.

Also for folder and Evaluation group, since no value exists, type `My Alerts` and press enter
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard4.png)

Scroll down and under `Add details for your alert rule` click on `Add annotation`, Choose `Summary` and enter the value as `{{ $labels.instance}} is reporting {{ $values.B.Value }} % memory usage for the last 5 minutes. Kindly review.`

Scroll to the top and click `Save and exit`. You will be taken to alerts page showing 1 rule and status normal.

![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard5.png)

## Slack setup

You need to create a slack app to receive notifications. Follow the link https://api.slack.com/apps to create an app.

![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard6.png)

Choose `From Scratch` and enter app name and choose workspace you are a part of.
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard7.png)

Click on Incoming Webhooks and turn on. Then click on Add new webhook to workspace.
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard8.png)
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard9.png)

Then choose the channel where alerts will be sent to. You can create a new channel in slack app if you want.
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard10.png)

Copy the webhook URL for use on grafana next page.
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard11.png)

## Grafana alert setup

Go back to grafana alerts page and click on `Contact points` and then `Add contact point`. Enter details as follows
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard12.png)

Name: `slack-alert`

Integration: `Slack`

Webhook URL: `COPIED FROM ABOVE`

Click on Test and then Send test notification. Make sure you are able to receive alert in Slack. Then you can save contact point.


## Change alerts to come to slack

Click on Notification policies and under Root Policy, click edit and choose default contact point to slack alert.
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard13.png)

At this time no alert will come since memory usage is below 70%. We can edit the alert and put a value like 20% so the alert is triggered. Also you can change evaluation time to 1m to make it alerted faster.
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard14.png)

Click save and exit and wait a while for alert to be evaluated. 
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard15.png)

You will receive an alert message like below.
![Create alert](https://raw.githubusercontent.com/gathecageorge/killercoda/main/3-grafana-alerting-create-dashboard/images/dashboard16.png)