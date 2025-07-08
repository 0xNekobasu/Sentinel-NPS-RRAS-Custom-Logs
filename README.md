# Sentinel-NPS-RRAS-Custom-Logs
A guide on how to configure the Azure monitor agent to pull the NPS/RRAS logs from windows server all parsed on ingestion. 

This in a sense has done the "hard" part of defining the JSON schema's for the custom logging but this is just essentially following Microsofts
own guide on how to ingest custom text logs via the Azure Monitor Agent. 
https://learn.microsoft.com/en-us/azure/azure-monitor/vm/data-collection-log-text#delimited-log-files

This guide was made partially because it was incredibly difficult to find any information on how other people had done this natively in Sentinel/Log Analytics. 
Some very useful links found are as follows:

[Interpreting NPS Logs](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771748(v=ws.10)?redirectedfrom=MSDN)


## Contents
This repo contains the following:

- A guide to setup the custom text logging for NPS logs for windows server.
- A JSON file containing the columns for making a custom table in Log Analytics.
- A transformation query which will parse the logs on ingestion. 

## Setup Collecting NPS Logs to Sentinel:
Link: [Guide to collect NPS logs](https://github.com/0xNekobasu/Sentinel-NPS-RRAS-Custom-Logs/tree/main/NPSLogs_CL)