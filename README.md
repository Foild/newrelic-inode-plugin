# Usage

Edit `config/newrelic_plugin.yml` and insert your hostname and license key where indicated.

Run the plugin:

```
./newrelic_inode_agent
```

Once you have the plugin installed and running, you can configure alerts.

# Configuring alerts

First, go to **Insights > Data Explorer > Metrics > Plugin: iNode Monitor**

Select your **host**, then select **Component**
Select the metric that you want. An example would be:

```
iNodes/dev/root/IUse.pct[IUse.pct]
```

On the graph drop-down, select "Average Value." You will get a graphed value.

You can verify you're looking at the correct data by comparing this value to the output of `df -i` on the host.

Save this component name somewhere, and add `Component/` to the front of it. This will be the component name you use for your metric.

Next, create the custom metric condition by going through the steps described here:

https://docs.newrelic.com/docs/alerts/new-relic-alerts/defining-conditions/define-custom-metrics-alert-condition#custom-metrics-threshold

Those steps are included below for reference:
> Next, go to **Alerts > Policies**.
> Select or create a policy, as you desire.
> Then, click **add a condition**.
> Select **Plugins**, click **iNode Monitor**
> Click **Next, Select Entities**.
> Check the box for the host you want to monitor.
> Click **Next, define thresholds**
> Under **When the target plugin instance**, select **Enter Metric Name**

For **metric name** enter your selected component name that you created earlier. For example:
```
Component/iNodes/dev/root/IUse.pct[IUse.pct]
```
Select **has a maximum value**, **above**
Enter `75`, or whatever your preference here is.
Name and save your new monitor.

**Testing**:

You can verify this works by entering an arbitrarily low number, such as 5. Wait the amount of time defined by your alert criteria, and verify that the alert triggers as expected. It appears you can also wildcard match, as the following worked for me:

```
Component/iNodes/dev/*/IUse.pct[IUse.pct]
```

I would recommend testing this to ensure that it works as expected, just in case there's a change in behavior.
