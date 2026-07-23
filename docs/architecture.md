# New-Y PowerApp — Architecture

## Overview

New-Y is a multi-role canvas Power App that manages the full sales cycle for a distribution business. It connects SharePoint Online lists (synced with Dynamics 365 Business Central) to deliver role-specific interfaces for Agents, Office staff, and Managers.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Microsoft Power Apps (Canvas) |
| Backend / Data | SharePoint Online Lists |
| ERP | Microsoft Dynamics 365 Business Central (on-prem) |
| Sync | PowerShell OData export (BC → SharePoint) |
| Automation | Power Automate (2 flows) |
| BI | Microsoft Power BI (embedded dashboard) |
| Auth | Microsoft 365 SSO + UserRoles SharePoint list |

## Architecture Diagram

```
Business Central (on-prem)
       │
       ▼  (PowerShell OData export)
SharePoint Lists (New-Y site)
  ├── Clienti-BC
  ├── Articoli-BC
  ├── Ordini-BC
  ├── RigheOrdini-BC
  ├── MovimentoArticoli-BC
  ├── Notifiche-BC
  ├── Riordini-BC
  └── UserRoles
       │
       ▼  (Power Apps connections)
PowerApp Canvas (8 screens)
  ├── LOGIN → SSO auth → role assignment
  └── HP-MANAGER → Manager hub
       │
       ▼  (Power Automate)
  Flow #1: Client Status Change → email to Manager
  Flow #2: Stock Traffic Light → reorder alert
```

## Role-Based Access

| Role | Home Page | Screens Available |
|------|-----------|-------------------|
| **Manager** | HP-MANAGER | Dashboard (Power BI), Orders, Customers, Reports, Products |
| **Office** | HP-MANAGER | Products, Orders, Customers |
| **Agent** | HP-MANAGER | Customers, Products |

## Screens (8 total)

Login → scrLoading → HP-MANAGER → Feature Screens

- `LOGIN` — SSO authentication, role lookup
- `scrLoading` — splash / init
- `HP-MANAGER` — role-based navigation hub
- `scrDashboard` — Power BI embedded (Manager)
- `scrOrders` — order management
- `scrCustomers` — customer management
- `scrProducts` — product catalog with stock filters
- `scrReports` — BI reports

## Theme System (TemaNewY)

| Token | Color | Usage |
|-------|-------|-------|
| ScuroPrimario | `#0D1B2A` | Main background |
| BluPrimario | `#1B3A5C` | Containers, headers |
| AccentiDorati | `#C9A227` | Gold accents |
| LuceSuperficie | `#F5F0EB` | Text on dark |
| TestoPrimario | `#0D1B2A` | Text on light |
| TestoSecondario | `#5A6B7D` | Secondary text |
| Successo | `#2E7D32` | Success states |
| Avvertimento | `#E65100` | Warning states |
| Errore | `#C62828` | Error states |
