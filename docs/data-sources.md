# Data Sources — SharePoint Lists & Business Central Sync

All data is stored in SharePoint Online lists on the New-Y site (`https://itsictpiemonte.sharepoint.com/sites/New-Y`) and synced from Microsoft Dynamics 365 Business Central (on-prem) via PowerShell OData export.

## SharePoint Lists

| List | BC Table | Purpose |
|------|----------|---------|
| UserRoles | — | Authentication & role mapping |
| Clienti-BC | Customer | Customer master data |
| Articoli-BC | Item | Product catalog |
| Ordini-BC | Sales Header | Sales orders |
| RigheOrdini-BC | Sales Line | Order lines |
| MovimentoArticoli-BC | Item Ledger Entry | Stock movements |
| Notifiche-BC | — | App notifications |
| Riordini-BC | — | Reorder requests |

## UserRoles Schema

| Field | Type | Description |
|-------|------|-------------|
| Title | String | Display name |
| Username (field_1) | Person | Microsoft 365 user |
| Role (field_2) | String | "Agente" / "Office" / "Manager" |
| Email | String | User email address |
| Nome e Cognome | String | Full name |
| Agent_Code | String | Agent identifier code |

## Sync Flow

```
Business Central (on-prem)
    │
    ▼  PowerShell OData export (scheduled)
SharePoint Lists
    │
    ▼  Power Apps native connectors
PowerApp Canvas
```

## Power Automate Flows

### Flow #1 — Client Status Change
Triggers when an Agent requests a status change on a customer. Sends an email notification to the Manager for approval.

### Flow #2 — Stock Traffic Light
Monitors stock levels. When an item drops below configurable thresholds, sends a reorder alert to the Manager with Warning (🟡) / Critical (🔴) levels.