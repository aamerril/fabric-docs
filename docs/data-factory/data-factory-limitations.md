---
title: Data Factory limitations overview
description: Identifies limitations that affect Data Factory in Microsoft Fabric features.
author: ssabat
ms.author: susabat
ms.topic: troubleshooting
ms.date: 03/07/2025
ms.custom: configuration
---

# Data Factory limitations overview

There are certain limitations to the current Data Factory in Microsoft Fabric features. Before submitting a support request, review the lists in this section to determine if you're experiencing a known limitation.

For service level outages or degradation notifications, check [Microsoft Fabric support](https://support.fabric.microsoft.com/).  

## Data pipeline limitations in Microsoft Fabric

The following list describes the current limitations of pipelines in Data Factory in Microsoft Fabric.

- Most of the Azure Data Factory copy and orchestration patterns are applicable to Fabric pipelines, but [tumbling window](/azure/data-factory/how-to-create-tumbling-window-trigger) isn't yet available.
-	Connectors don't support OAuth and  Azure key vault (AKV).
-	Managed System Identity (MSI) is only available for Azure Blob Storage. Support for other sources is coming soon. 
-	Connectors can't use parameters.
-	GetMetaData activity can't have a source from Fabric KQL databases.
-	Script activity can't have a source from Fabric KQL databases.
-	Copy activity uses a Web connector, whereas Web/Webhook activities use a Web v2 connector that supports richer functionality, like audience and resource URI.
-	Validation activity, Mapping Data Flow activity, and the SSIS integration runtime aren't available. 
-	Pipelines can't use a managed virtual network.
-	Web activity doesn't support service principal based authentication.
-	Pipeline scheduling options currently include only by the minute, hourly, daily, and weekly.
-	Dataflow Gen2 (CI/CD, preview) is currently not supported in pipeline as activity. 
-	Background sync of authentication doesn't happen for pipelines. Recommendation is to do minor description like updates to pipelines and save them. That way, new token is obtained and cached so pipeline can run again with updated password of entra id. 

## Data pipeline resource limits

The following table describes the resource limitations for pipelines in Data Factory in Microsoft Fabric.

| Pipeline Resource | Default limit | Maximum limit |
|---|---|---|
| Total number of pipelines within a workspace | 5,000 | 5,000 |
| Concurrent pipeline runs per workspace that's shared among all pipelines in workspace  | 10,000 | 10,000 |
| External activities like stored procedure, Web, Web Hook, and others | 3,000 | 3,000 |
| Pipeline activities execution for Lookup, GetMetadata, and Delete | 1,000 | 1,000 |
| Concurrent authoring operations, including test connection, browse folder list and table list, preview data, and so on | 200 | 200 |
| Maximum activities per pipeline, which includes inner activities for containers | 120 | 120 |
| Maximum parameters per pipeline | 50 | 50 |
| ForEach items | 100,000 | 100,000 |
| ForEach parallelism | 20 | 50 |
| Lookup Activity item count | 5000 | 5000 |
| Maximum queued runs per pipeline | 100 | 100 |
| Characters per expression | 8,192 | 8,192 |
| Maximum timeout for pipeline activity runs | 24 hours | 24 hours |
| Bytes per object for pipeline objects | 200 KB | 200 KB |
| Bytes per payload for each activity run | 896 KB | 896 KB |
| Intelligent throughput optimization per copy activity run | Auto | 256 |
| Concurrent intelligent throughput optimization per workspace | 400 | 400 |
| Meta Data Entity Size limit in a factory | 2 GB | 2 GB |

## Data Factory Dataflow Gen2 limitations

The following list describes the limitations for Dataflow Gen2 in Data Factory in Microsoft Fabric.

- Data destination to Lakehouse:
  - Spaces or special characters aren't supported in column or table names.
  - Duration and binary columns aren't supported while authoring Dataflow Gen2 dataflows.
- You must have a [currently supported gateway installed](/data-integration/gateway/service-gateway-monthly-updates) to use with Dataflow Gen2. At minimum, Dataflow Gen2 supports the last six released gateway versions.
- When you use OAuth2 credentials, the gateway currently doesn't support refreshes longer than an hour. These refreshes fail because the gateway can't support refreshing tokens automatically when access tokens expire, which happens one hour after the refresh started. If you get the errors "InvalidConnectionCredentials" or "AccessUnauthorized" when accessing cloud data sources using OAuth2 credentials even though the credentials have been updated recently, you may be hitting this error. This limitation for long running refreshes exists for both VNET gateways and on-premises data gateways.
- The Delta Lake specification doesn't support case sensitive column names, so `MyColumn` and `mycolumn`, while supported in Mashup, results in a "duplicate columns" error.
- Currently, column nullability is defaulting to allow nulls in all columns in the destination.
- After you save/publish your dataflow gen2 we require the validation/publish process to finish within 10 minutes per query. If you exceed this 10 minute limit try to simplify your queries or split your queries in dataflow gen2. 
- You can't connect to a public endpoint of an Azure Storage account using Power Query Online or Dataflow Gen2 (no gateway) if the Azure Storage account already has one or more Private Endpoints created. You need to connect to such storage accounts using a VNet data gateway or an on-premises data gateway that can connect using private endpoints.
- Dataflow Gen2 doesn't support for guest users in the tenant to connect to the data sources and destinations in the tenant the user is guest. Use a native user in the tenant to connect to the data sources and destinations.
- Consuming data from a dataflow gen2 with the dataflow connector requires Admin, Member or Contributor permissions. Viewer permission isn't sufficient and isn't supported for consuming data from the dataflow.

The following table indicates the supported data types in specific storage locations.

| **Supported data types per storage location:**  | DataflowStagingLakehouse | Azure DB (SQL) Output | Azure Data Explorer Output | Fabric Lakehouse (LH) Output | Fabric Warehouse (WH) Output |
|-------------------------------------------------|--------------------------|-----------------------|----------------------------|------------------------------|------------------------------|
| Action| No| No | No  | No    | No    |
| Any   | No| No | No  | No    | No    |
| Binary| No| No | No  | No    | No    |
| Currency | Yes   | Yes| Yes | Yes   | No    |
| DateTimeZone| Yes   | Yes| Yes | No    | No    |
| Duration | No| No | Yes | No    | No    |
| Function | No| No | No  | No    | No    |
| None  | No| No | No  | No    | No    |
| Null  | No| No | No  | No    | No    |
| Time  | Yes   | Yes| No  | No   | No   |
| Type  | No| No | No  | No    | No    |
| Structured (List, Record, Table)| No| No | No  | No    | No    |

## Related content

- [Service level outages](https://support.fabric.microsoft.com)
- [Get your questions answered by the Data Factory community](https://community.fabric.microsoft.com/t5/Data-Factory-preview-Community/ct-p/datafactory)
