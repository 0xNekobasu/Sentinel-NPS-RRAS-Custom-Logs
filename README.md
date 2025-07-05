# Sentinel-NPS-RRAS-Custom-Logs
A guide on how to configure the Azure monitor agent to pull the NPS logs from windows server all parsed on ingestion. 

This in a sense has done the "hard" part of defining the JSON schema's for the custom logging but this is just essentially following Microsofts
own guide on how to ingest custom text logs via the Azure Monitor Agent. 
https://learn.microsoft.com/en-us/azure/azure-monitor/vm/data-collection-log-text#delimited-log-files

This also serves as beginner friendly guide to hopefully make the use of the Azure Monitor agent easier for those who are just starting to use it for ingesting custom log sources.

## Contents
This repo contains the following:

- A guide to setup the custom text logging for NPS logs for windows server.
A JSON file containing the columns for making a custom table in Log Analytics.

## Setup Collecting NPS Logs to Sentinel:
Link: 