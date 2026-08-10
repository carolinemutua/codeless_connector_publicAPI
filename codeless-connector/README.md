# Codeless connector: FeodoTracker threat intelligence

A single ARM template (`codeless-connector.json`) that deploys a complete Microsoft Sentinel codeless connector against the public [FeodoTracker](https://feodotracker.abuse.ch/) botnet command-and-control feed from abuse.ch. It is fully self-contained: it creates every resource it needs, so there are no prerequisites to satisfy first.

## What it deploys

The template creates seven resources, described in the `comments` field of each one:

1. **Data Collection Endpoint** — the HTTPS ingestion endpoint data flows into.
2. **Custom table** (`FeodoTracker_BotnetC2_CL`) — the schema for the botnet C2 records.
3. **Data Collection Rule** — transforms the API fields (snake_case) into the table columns (PascalCase) with a KQL transform.
4. **Connector UI definition** — the tile shown under Sentinel, Data connectors.
5. **RestApiPoller data connector** — the engine that calls the FeodoTracker API roughly every five minutes and pushes rows through the endpoint and rule into the table.
6. **Scheduled analytics rule** — an hourly detection that cross-references `CommonSecurityLog` against currently online C2 IPs and raises High severity incidents.
7. **Workbook** — a dashboard with a malware-family pie chart, a top hosting-countries bar chart, and a live table of online C2 servers.

## Parameters

| Parameter | Default | Purpose |
|---|---|---|
| `workspace` | `sentineldatalake` | The Log Analytics workspace where Sentinel is enabled. |
| `connectorDefinitionName` | `FeodoTrackerThreatIntelConnector` | Links the UI tile to the poller. Both must match exactly. |
| `dataCollectionRuleName` | `FeodoTrackerDCR` | Name of the DCR resource. |
| `dataCollectionEndpointName` | `codelessconnectorpoll` | Name of the DCE the template creates. |
| `location` | `West Europe` | Must match the workspace region. |

The defaults are generic lab names. Change `workspace` to your own workspace name before deploying.

## Deploy

Deploy it into your own Sentinel-enabled workspace with the Azure CLI:

**Windows (PowerShell):**
```powershell
az deployment group create `
  --resource-group <your-rg> `
  --template-file codeless-connector.json `
  --parameters workspace=<your-workspace>
```

**macOS / Linux (bash/zsh):**
```bash
az deployment group create \
  --resource-group <your-rg> \
  --template-file codeless-connector.json \
  --parameters workspace=<your-workspace>
```

The connector needs no authentication: the FeodoTracker feed is public. The poller sends a dummy `X-Source` header only because the Sentinel RestApiPoller does not accept an auth type of None. The feed ignores the header.

## Data source

[FeodoTracker](https://feodotracker.abuse.ch/) by [abuse.ch](https://abuse.ch/) publishes botnet C2 IP addresses for malware families such as Emotet, QakBot, and IcedID. Review their terms before relying on the feed.
