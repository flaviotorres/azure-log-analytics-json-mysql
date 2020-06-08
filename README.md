# Azure Log Analytics OMS - Custom metrics for MySQL


Collecting custom metrics from MySQL and sending to Azure Log Analytics thru oms agent fluentd custom JSON data sources

How to send custom metrics from your MySQL on-prem to Azure Log Analytics.

NOTE: Considering that you already have Azure Log Analytics workspace setup and OMS agent installed and running on your vm.
- Azure Monitor: https://azure.microsoft.com/en-us/services/monitor/
- OMS Agent for Linux: https://github.com/microsoft/OMS-Agent-for-Linux  


I decided to use JSON data as it is already supported by OMS Agent, all you have to do is to configure FluentD plugin as described here:
- Collecting custom JSON data sources with the Log Analytics agent for Linux in Azure Monitor: https://docs.microsoft.com/en-us/azure/azure-monitor/platform/data-sources-json


The only difference is that your command will be:


```
command '/usr/local/bin/mysql_info.py'
```

Please refer to https://github.com/flaviotorres/azure-log-analytics-json-mysql/blob/master/mysql_info.py file and make sure linux owner and group is set to omsagent:omiusers

Once you have the script setup, run and you will see get something like:

```
{"active_connections": 12}
```

Yeah, right now this is the only one I have exported, MySQL OMS agente already have something for MySQL Slow and Error Logs.

NOTE: remind to check the log file (/var/opt/microsoft/omsagent/WORKSPACE_ID/log/omsagent.log) after restarting the agent. 

Alright, now you should see in Log Analytics Custom Logs a table called MySQL_CL. Here are the queries for basic monitoring.


```
// MySQL active connections
MySQL_CL 
| project TimeGenerated, active_connections_d
| render timechart
```


Result:
Azure_Log_Analytics_onprem_MySQL_Dashboard.png
![Log Analytics Dashboard](https://github.com/flaviotorres/azure-log-analytics-json-mysql/blob/master/Azure_Log_Analytics_onprem_MySQL_Dashboard.png?raw=true)




