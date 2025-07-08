# NPS Logs Custom Log Ingestion Setup

## Information
This guide will go through the steps of setting up the NPS logs to be "ingestable" on the Windows Server. It also has the JSON schema for setting up the custom log table and also the transformation query which parses the raw data into the correct columns. 

### Pre requisites
You will need the ability to assign roles in Azure, modify Log analytics workspaces and create DCR's and DCE's.
Following the principle of least privilege you will need a concotion of:
- Log analytics Contributor 
- Monitoring Contributor

Or if not following the principle of least privilege, Owner/Contributor of the Azure subscription will do. 

## Instructions

1. Login to your NPS/RRAS/RADIUS servers and open up "Network Policy Server".

2. Click on "Acounting" and then select "Configure Accounting"

3. Select "Log to a text file", click next

4. Ensure all the tickboxes are "ticked":
- Accounting Requests
- Authentication Requests
- Periodic Accounting Status
- Periodic Authentication Status

5. Leave the log file directory as default (this just makes it better for "at scale" operations, if someone forgets to change the log directory for one server then you wont miss any logs)

6. Tick or leave unticked "Logging failure action" in enterprise settings you probably want this ticked. 

7. Complete the wizard, next select "change Log file properties".

8. In the pop-up menu, select the tab named "Log file" and set the format to "ODBC (Legacy)". 
This is especially important unless you DON'T want the transformation query and custom table to function as according to this guide.

9. Setup Custom Table in Log Analytics using the following guide and the following templates linked:
https://learn.microsoft.com/en-us/azure/sentinel/connect-custom-logs-ama?tabs=portal

- Run the following powershell and login to the tenant which you wish to create the custom table, you will need Log Analytics Contributor at minimum to do this:
`Connect-AzAccount`

- Set a variable which contains the table parameters as found in NPSLogs_CL.json [NPSLogs_CL.json](https://github.com/0xNekobasu/Sentinel-NPS-RRAS-Custom-Logs/blob/main/NPSLogs_CL/NPSLogs_CL.json) or use:
```
$VARIABLENAME = @'
{
    "properties": {
        "schema": {
               "name": "NPSLogs_CL",
               "columns": [
                    {
                        "name": "TimeGenerated",
                        "type": "DateTime"
                    }, 
                    {
                        "name": "ComputerName",
                        "type": "string"
                    },
                    {
                        "name": "ServiceName",
                        "type": "string"
                    },
                    {
                        "name": "Date",
                        "type": "string"
                    },
                    {
                        "name": "Time",
                        "type": "string"
                    },
                    {
                        "name": "PacketType",
                        "type": "int"
                    },
                    {
                        "name": "UserName",
                        "type": "string"
                    },
                    {
                        "name": "FQDN",
                        "type": "string"
                    },
                    {
                        "name": "CalledStationID",
                        "type": "string"
                    },
                    {
                        "name": "CallingStationID",
                        "type": "string"
                    },
                    {
                        "name": "CallbackNumber",
                        "type": "string"
                    },
                    {
                        "name": "FramedIPAddress",
                        "type": "string"
                    },
                    {
                        "name": "NASIdentifier",
                        "type": "string"
                    },
                    {
                        "name": "NASIPAddress",
                        "type": "string"
                    },
                    {
                        "name": "NASPort",
                        "type": "int"
                    },
                    {
                        "name": "ClientVendor",
                        "type": "int"
                    },
                    {
                        "name": "ClientIPAddress",
                        "type": "string"
                    },
                    {
                        "name": "ClientFriendlyName",
                        "type": "string"
                    },
                    {
                        "name": "EventTimestamp",
                        "type": "DateTime"
                    },
                    {
                        "name": "PortLimit",
                        "type": "int"
                    },
                    {
                        "name": "NASPortType",
                        "type": "int"
                    },
                    {
                        "name": "ContactInfo",
                        "type": "string"
                    },
                    {
                        "name": "FramedProtocol",
                        "type": "int"
                    },
                    {
                        "name": "ServiceType",
                        "type": "int"
                    },
                    {
                        "name": "AuthenticationType",
                        "type": "int"
                    },
                    {
                        "name": "PolicyName",
                        "type": "string"
                    },
                    {
                        "name": "ReasonCode",
                        "type": "int"
                    },
                    {
                        "name": "Class",
                        "type": "string"
                    },
                    {
                        "name": "SessionTimeout",
                        "type": "int"
                    },
                    {
                        "name": "IdleTimeout",
                        "type": "int"
                    },
                    {
                        "name": "TerminationAction",
                        "type": "int"
                    },
                    {
                        "name": "EAPFriendlyName",
                        "type": "string"
                    },
                    {
                        "name": "AcctStatusType",
                        "type": "int"
                    },
                    {
                        "name": "AcctDelayTime",
                        "type": "int"
                    },
                    {
                        "name": "AcctInputOctets",
                        "type": "int"
                    },
                    {
                        "name": "AcctOutputOctets",
                        "type": "int"
                    },
                    {
                        "name": "AcctSessionId",
                        "type": "string"
                    },
                    {
                        "name": "AcctAuthentic",
                        "type": "int"
                    },
                    {
                        "name": "AcctSessionTime",
                        "type": "int"
                    },
                    {
                        "name": "AcctInputPackets",
                        "type": "int"
                    },
                    {
                        "name": "AcctOutputPackets",
                        "type": "int"
                    },
                    {
                        "name": "AcctTerminateCause",
                        "type": "int"
                    },
                    {
                        "name": "AcctMultiSsnID",
                        "type": "string"
                    },
                    {
                        "name": "AcctLinkCount",
                        "type": "int"
                    },
                    {
                        "name": "AcctInterimInterval",
                        "type": "int"
                    },
                    {
                        "name": "TunnelType",
                        "type": "int"
                    },
                    {
                        "name": "TunnelMediumType",
                        "type": "int"
                    },
                    {
                        "name": "TunnelClientEndpt",
                        "type": "string"
                    },
                    {
                        "name": "TunnelServerEndpt",
                        "type": "string"
                    },
                    {
                        "name": "AcctTunnelConn",
                        "type": "string"
                    },
                    {
                        "name": "TunnelPvtGroupID",
                        "type": "string"
                    },
                    {
                        "name": "TunnelAssignmentID",
                        "type": "string"
                    },
                    {
                        "name": "TunnelPreference",
                        "type": "int"
                    },
                    {
                        "name": "MSAcctAuthType",
                        "type": "int"
                    },
                    {
                        "name": "MSAcctEAPType",
                        "type": "int"
                    },
                    {
                        "name": "MSRASVersion",
                        "type": "string"
                    },
                    {
                        "name": "MSRASVendor",
                        "type": "int"
                    },
                    {
                        "name": "MSCHAPError",
                        "type": "string"
                    },
                    {
                        "name": "MSCHAPDomain",
                        "type": "string"
                    },
                    {
                        "name": "MSMPPEEncryptionTypes",
                        "type": "int"
                    },
                    {
                        "name": "MSMPPEEncryptionPolicy",
                        "type": "int"
                    },
                    {
                        "name": "ProxyPolicyName",
                        "type": "string"
                    },
                    {
                        "name": "ProviderType",
                        "type": "int"
                    },
                    {
                        "name": "ProviderName",
                        "type": "string"
                    },
                    {
                        "name": "RemoteServerAddress",
                        "type": "string"
                    },
                    {
                        "name": "MSRASClientName",
                        "type": "string"
                    },
                    {
                        "name": "MSRASClientVersion",
                        "type": "int"
                    },
                    {
                        "name": "FilePath",
                        "type": "string"
                    },
                    {
                        "name": "RawData",
                        "type": "string"
                    }
              ]
        }
    }
}
'@
```
- Run the following but modify it to suit your Azure environment:
```
Invoke-AzRestMethod -Path "/subscriptions/{subscriptionID}/resourcegroups/{resourcegroup}/providers/microsoft.operationalinsights/workspaces/{Workspace}/tables/{TableName}_CL?api-version=2021-12-01-preview" -Method PUT -payload $VARIABLEFROM ABOVE
```

10. You will need a Data Collection Endpoint to ingest Custom text file logs. Create one of these if you don't have one already.

11. Create Data Collection Rule to collect these logs. You can only have 1 set of custom text logs per DCR so call this something like `<tenant Identifier>-DCR-NPSLogs`
Ensure you select a Data Collection Endpoint on the creation of the DCR.
Target any NPS/RRAS machines you have.

12. When adding your datasource set the following:

| Setting | Value |
|---------|-------|
| Data Source type | Custom Text Logs |
| File Pattern | C:\Windows\system32\LogFiles\IN*.log |
| Table Name | NPSLogs_CL |
| Record Delimiter | Timestamp |
| Timestamp Format | ISO 8601 |

This transformation query will pull all the logs though into the custom table and sort them into the columns you setup earlier.
See "NPSLogs_CL-Transformation.kql" link: [NPSLogs_CL-Transformation.kql](https://github.com/0xNekobasu/Sentinel-NPS-RRAS-Custom-Logs/blob/main/NPSLogs_CL/NPSLogs_CL-Transformation.kql)

13. Once deployed, the logs should appear in your custom table after about 5 minutes.
