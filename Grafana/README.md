# GRAFANA 

## CONNECTING GRAFANA TO PROMETHEUS
- Go to `Home > Connections > Data sources > Add a new data source > Prometheus` and add the corresponding details

## Grafana files
* **graphana.ini:**
   - You can find `grafana.ini` file in `/etc/grafana/` folder
   - This file contains important details like where the log files are located and port information and every other important configuration
   - It is a best practice to copy this file with `custom.ini` name in the same folder if you need to modify the configuration

* **datasources.yml:**
   - You can manage data sources in Grafana by adding YAML configuration files in the `provisioning/datasources` directory
   - Each config file can contain a list of datasources to add or update during startup. If the data source already exists, Grafana reconfigures it to match the provisioned configuration file
   - Reference: https://grafana.com/docs/grafana/latest/administration/provisioning/

## Alerting

- Alerts are raised when a defined rule is violated
- Rules are defined as queries and are checked by Alert Manager
- Alerts are sent to Notification Policies
- Notification policies send notifications to contact points

  ![alt text](alert.png)
- Set up an alert by going to `Home > Alerting > Alert rules > New alert rule`

## Alert Notifications

- Configure SMTP in `grafana.ini` file
- Under `[smtp]` in this file, add values for `enabled`, `host`, `password`, and `user` of of SMTP
- Once done, restart Grafana
- Add Contact details by navigating to `Home > Alerting > Contact points`

## Notification Policy

- You need to set up a notification policy in order to use a contact point
- Navigate to `Home > Alerting > Notification policies > New child policy`
- Use the labels used in the alert rule or if you want to send notifications based on metric name, put label as `__name__` and value as the `<metric_name>`
- Now select the `contact point` you are creating the notification policy for

## Sending alerts to Slack from Grafana

- From your slack workspace, go to the channel where you want to send the notification and click on the drop down close to the name of the channel
- Go to the `Integration > Add an App`
- In the search box, search for `webhook` and install `Incoming WebHooks`
- Then on the webpage, select `Add to Slack` and select your channel where you want your alerts to go
- Click on `Add Incoming WebHooks integration`
- Copy the `Webhook URL`
- Now navivate to Grafana and add new contact point
- Select `Slack` as the integration in the contact point
- Add `channel name` under `Receipient`
- You either need to pass a token or a webhook url
- Add the webhook url that we copied earlier and click on `Test` and it should send a test alert to your slack channel
- Create corresponding notification policy and attach it to the respective alert

## Silencing the alerts

- If you do not want an alert to be sent in a particular time window, then silence that alert for that time window
- Navigate to `Home > Alerting > Silences > Add silence`
- Set start and end time as per your requirement and add labels matching the alert you want to silent