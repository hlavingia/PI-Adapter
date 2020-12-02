---
uid: SystemAndAdapterConfiguration1-4
---

# System and adapter configuration

You can configure the system component and adapter component together using a single file.

## Change system and adapter configuration

Change the system and adapter configuration by importing the JSON file using a REST client:

1. Using any text editor, create a file that contains the System and adapter configuration in the JSON format.

    - For content structure, see [Example](#example).

2. Save the file. For example,  `ConfigureSystemAndAdapter.json`.

3. Use any of the [Configuration Tools](xref:ConfigurationTools1-4) capable of making HTTP requests and run a PUT command with the contents of the file to the following endpoint: `http://localhost:5590/api/v1/configuration`

    **Note:** `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    **Note:** Run this command from the same directory where the file is located.

    ```bash
    curl -d "@ConfigureSystemAndAdapter.json" -H "Content-Type: application/json" -X  PUT "http://localhost:5590/api/v1/configuration"
    ```

    **Note:** In order for some of the adapter specific configurations to take effect, you have to restart the adapter.

    If the operation fails due to errors in the configuration, the count of the error and suitable error messages are returned in the result.

## Example

**Note**: The following is an example configuration; it does not necessarily represent the configuration of the adapter that you are currently using. The data source and data selection configurations are different for every adapter.

<details>
    <summary>Sample OPC UA adapter configuration</summary>
    <pre>
{
    "OpcUa1": {
        "Logging": {
            "logLevel": "Information",
            "logFileSizeLimitBytes": 34636833,
            "logFileCountLimit": 31
        },
        "DataSource": {
            "EndpointUrl": "opc.tcp://OPCUAServerEndpoint/OPCUA/Server",
            "UseSecureConnection": false,
            "StreamPrefix": "OPC_Prefix_",
            "UserName": null,
            "Password": null,
            "RootNodeIds": null,
            "IncomingTimestamp": "Source",
            "applyPrefixToStreamId": true
        },
        "DataSelection": [
            {
                "Selected": true,
                "Name": "Sawtooth",
                "NodeId": "ns=3;s=Sawtooth",
                "StreamId": "SawtoothStream"
            }
        ]
    },
    "System": {
        "Logging": {
            "logLevel": "Information",
            "logFileSizeLimitBytes": 34636833,
            "logFileCountLimit": 31
        },
        "HealthEndpoints": [
        ],
    "Diagnostics": {
            "enableDiagnostics": true
    },
        "Components": [
            {
                "componentId": "Egress",
                "componentType": "OmfEgress"
            },
            {
                "componentId": "OpcUa1",
                "componentType": "OpcUa"
            }
        ],
    "Buffering": {
            "BufferLocation": "C:/ProgramData/OSIsoft/Adapters/OpcUa/Buffers",
            "MaxBufferSizeMB": -1,
            "EnableBuffering": true
        }
     },
    "OmfEgress": {
        "Logging": {
            "logLevel": "Information",
            "logFileSizeLimitBytes": 34636833,
            "logFileCountLimit": 31
        },
        "DataEndpoints": [
            {
                "id": "WebAPI EndPoint",
                "endpoint": "https://PIWEBAPIServer/piwebapi/omf",
                "userName": "USERNAME",
                "password": "PASSWORD"
            },
            {
                "id": "OCS Endpoint",
                "endpoint": "https://OCSEndpoint/omf",
                "clientId": "CLIENTID",
                "clientSecret": "CLIENTSECRET"
            },
            {
                "Id": "EDS",
                "Endpoint": "http://localhost:<port_number>/api/v1/tenants/default/namespaces/default/omf"
                "UserName": "eds",
                "Password": "eds"
            }
        ]
    }
}
   </pre>
</details>

## REST URLs

| Relative URL          | HTTP verb | Action                                            |
| --------------------- | --------- | ------------------------------------------------- |
| api/v1/configuration/ | PUT       | Replaces the configuration for the entire adapter |