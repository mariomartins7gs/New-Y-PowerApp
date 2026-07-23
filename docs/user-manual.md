# New-Y PowerApp

User Manual v1.0 — Final

23 July 2026

**⚠️ Disclaimer / Avviso**
This document describes New-Y PowerApp (final delivered version) as of 23 July 2026.
The application is delivered and functional.

Questo documento descrive New-Y PowerApp (versione finale consegnata) al 23 luglio 2026.
L'applicazione è stata consegnata ed è funzionante.

## Table of Contents / Indice

1. [1. Introduction / Introduzione](#introduction)
2. [2. Getting Started / Avvio](#getting-started)
3. [3. Role-Based Access / Accesso Basato sul Ruolo](#role-based-access)
4. [4. Screen Guide / Guida Schermate](#screen-guide)
5. [5. Data Sources / Fonti Dati](#data-sources)
6. [6. Known Issues & Limitations / Note Problemi & Limitazioni](#known-issues)
7. [7. Appendix — Brand Guidelines / Appendice — Linee Guida Brand](#appendix)

## 1. Introduction / Introduzione

### EN

**New-Y PowerApp** is a canvas application built on Microsoft Power Apps, designed to manage a sales cycle workflow for the New-Y project. It connects to SharePoint lists and Microsoft Dynamics 365 Business Central to provide a unified interface for Agents, Office staff, and Managers.

Key capabilities:

* Role-based authentication (Agent / Office / Manager)
* Customer management linked to Business Central
* Order creation, tracking, and reporting
* Stock checking and product catalog browsing
* Real-time notifications

### IT

**New-Y PowerApp** è un'applicazione canvas costruita con Microsoft Power Apps, progettata per gestire il workflow del ciclo di vendita per il progetto New-Y. Si connette a liste SharePoint e Microsoft Dynamics 365 Business Central per fornire un'interfaccia unificata per Agenti, personale Office e Manager.

Funzionalità principali:

* Autenticazione basata sul ruolo (Agente / Office / Manager)
* Gestione clienti collegata a Business Central
* Creazione, tracciamento e reporting ordini
* Controllo stock e navigazione catalogo prodotti
* Notifiche in tempo reale

## 2. Getting Started / Avvio

### Access

1. Open the PowerApp via the SharePoint portal or direct link
2. You will be redirected to the **LOGIN** screen
3. Sign in with your Microsoft 365 / school account (SSO)
4. The app will look up your email in the **UserRoles** SharePoint list
5. Your role (Agent, Office, or Manager) determines your access level

### First Login

On first access, the app initializes global variables:

```
Set(TemaNewY, {
    ScuroPrimario:    #0D1B2A,   // deep navy background
    BluPrimario:      #1B3A5C,   // medium blue containers
    AccentiDorati:    #C9A227,   // gold accents
    LuceSuperficie:   #F5F0EB,   // off-white text
    ...
});
Set(varRuoliAmmessi, ["Agente", "Office", "Manager"]);
Set(varRuoloUtente, LookUp(UserRoles, Email = User().Email, Role));
```

## 3. Role-Based Access / Accesso Basato sul Ruolo

The application has **three roles**, all through a single **HP-MANAGER** hub:

| Role | Home Page | Access |
| --- | --- | --- |
| **Manager** | HP-MANAGER | Dashboard (Power BI), Orders, Customers, Reports, Products |
| **Office** | HP-MANAGER | Products, Orders, Customers |
| **Agent** (Agente) | HP-MANAGER | Customers, Products |

**Role Switching (Manager only)**: The Manager role can access all sections from the hamburger menu. Office and Agent roles are restricted to their respective sections.

## 4. Screen Guide / Guida Schermate

### 4.1 LOGIN

**Purpose:** Authenticate the user and assign their role
**Access:** All users — entry point of the application
**Key elements:** New-Y branding, email field (pre-filled from SSO), Sign-In button, error message label
**Status:** ✅ Functional

### 4.2 scrLoading

**Purpose:** Splash / loading screen shown during app initialization
**Access:** All users (automatic, transitional)
**Status:** ✅ Functional

### 4.3 HP-MANAGER

**Purpose:** Main navigation hub — role-based menu displayed after login
**Access:** All users (Manager sees all sections, Agent/Office see only their section)
**Status:** ✅ Functional

### 4.4 scrDashboard

**Purpose:** Manager-only dashboard with embedded Power BI reports
**Access:** Manager
**Status:** ✅ Functional

### 4.5 scrOrders

**Purpose:** View and manage all orders
**Access:** Manager, Office
**Status:** ✅ Functional

### 4.6 scrCustomers

**Purpose:** Customer management — view all customers or search
**Access:** All (Manager sees all, Agent sees assigned)
**Status:** ✅ Functional

### 4.7 scrProducts

**Purpose:** Product catalog browsing with stock filters
**Access:** All users
**Status:** ✅ Functional

### 4.8 scrReports

**Purpose:** Business intelligence and reporting
**Access:** Manager
**Status:** ✅ Functional

## 5. Data Sources / Fonti Dati

All data sources are SharePoint lists on the site `https://itsictpiemonte.sharepoint.com/sites/New-Y`, synced with Microsoft Dynamics 365 Business Central.

| List Name | SharePoint ID | Business Central Table | Purpose |
| --- | --- | --- | --- |
| UserRoles | abb48815-... | — | Authentication & role mapping |
| Clienti-BC | f191f69d-... | Customer | Customer master data |
| Articoli-BC | 35a706f5-... | Item | Product catalog |
| Ordini-BC | 5c252e75-... | Sales Header | Sales orders |
| RigheOrdini-BC | 478f0fcb-... | Sales Line | Order lines |
| MovimentoArticoli-BC | 5f90955d-... | Item Ledger Entry | Stock movements |
| Notifiche-BC | 78b8d90a-... | — | App notifications |
| Riordini-BC | b5ecbb0a-... | — | Reorder requests |

### UserRoles Schema

| Field | Type | Description |
| --- | --- | --- |
| Title | String | Display name |
| Username (field\_1) | Person | Microsoft 365 user |
| Role (field\_2) | String | "Agente" / "Office" / "Manager" |
| Email | String | User email address |
| Nome e Cognome | String | Full name |
| Agent\_Code | String | Agent identifier code |

### Sync Architecture

```
Business Central (BC)
        ↓ (OData export via PowerShell)
SharePoint Lists (New-Y site)
        ↓ (PowerApps connections)
PowerApp (canvas)
```

*Note: The BC → SharePoint sync is handled by a separate Power Automate flow / PowerShell script (see sync-bc/ in project docs).*

## 6. Known Issues & Limitations / Note Problemi & Limitazioni

| # | Issue | Severity | Status |
| --- | --- | --- | --- |
| 1 | Internal SharePoint column names (field\_1, field\_2) visible in data schema — should use friendly names | Low | Visual only |
| 2 | Sample data sources (DropDownSample, CustomGallerySample) still present in app | Low | Clean up |

## 7. Appendix / Appendice — Brand Guidelines

### Color Palette (TemaNewY)

| Token | Hex | Usage | Preview |
| --- | --- | --- | --- |
| ScuroPrimario | #0D1B2A | Deep navy — main background |  |
| BluPrimario | #1B3A5C | Medium blue — containers, headers |  |
| AccentiDorati | #C9A227 | Gold — accent labels, highlights |  |
| LuceSuperficie | #F5F0EB | Off-white — text on dark backgrounds |  |
| TestoPrimario | #0D1B2A | Text on light backgrounds |  |
| TestoSecondario | #5A6B7D | Secondary/muted text |  |
| Successo | #2E7D32 | Green — success states |  |
| Avvertimento | #E65100 | Orange — warning states |  |
| Errore | #C62828 | Red — error states |  |

### Typography

| Element | Font | Weight | Size |
| --- | --- | --- | --- |
| App title | Segoe UI | Bold | 30pt |
| Screen header | Segoe UI | Bold | 20-25pt |
| Body text | Open Sans / Segoe UI | Regular | 13-15pt |
| Button text | Open Sans | Regular/Bold | 13pt |
| Label text | Segoe UI | Regular | 12-13pt |

### Logo

File: `new-y_logo_sign.png` (1060×1008 px, PNG with alpha)
Location: `docs/brand/new-y_logo_sign.png`
Usage: Header, cover page, login screen

New-Y PowerApp User Manual — © 2026 New-Y Project — Confidential
Version v1.0 · 23 July 2026
