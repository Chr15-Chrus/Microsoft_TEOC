# Microsoft Teams Emergency Operations Center (TEOC)
## Comprehensive Architecture Documentation

---

## Table of Contents

1. [Overview](#overview)
2. [Architecture Diagram](#architecture-diagram)
3. [Architecture Layers](#architecture-layers)
4. [Core Components](#core-components)
5. [Technology Stack](#technology-stack)
6. [Data Flow](#data-flow)
7. [Deployment Pipeline](#deployment-pipeline)
8. [Repository Structure](#repository-structure)

---

## Overview

**Microsoft Teams Emergency Operations Center (TEOC)** is an enterprise-grade solution template built on the Microsoft 365 platform. It centralizes incident response, information sharing, and field communications by leveraging powerful services like Microsoft Teams, SharePoint, and Microsoft Lists.

### Key Characteristics
- ✅ Cloud-native architecture on Azure
- ✅ Open-source (MIT License)
- ✅ Enterprise-grade security
- ✅ Highly extensible and customizable
- ✅ Fully integrated with Microsoft 365
- ⚠️ Community-supported as of March 31, 2025

---

## Architecture Diagram

```
╔════════════════════════════════════════════════════════════════════════════════════════╗
║                   MICROSOFT TEAMS EMERGENCY OPERATIONS CENTER (TEOC)                    ║
║                              Architecture Diagram                                        ║
╚════════════════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              END USER LAYER (Clients)                                    │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                          │
│   ┌──────────────┐          ┌──────────────┐          ┌──────────────┐                  │
│   │  Microsoft   │          │  SharePoint  │          │    Teams     │                  │
│   │   Teams App  │          │  Team Site   │          │   Desktop    │                  │
│   │   (Browser)  │          │   (Browser)  │          │   (Client)   │                  │
│   └──────────────┘          └──────────────┘          └──────────────┘                  │
│                                                                                          │
└─────────────────────────────────────────────────────────────────────────────────────────┘
                ▲                    ▲                       ▲
                │                    │                       │
       ┌────────┴────────┐  ┌────────┴────────┐  ┌──────────┴─────────┐
       │                 │  │                 │  │                    │
       ▼                 ▼  ▼                 ▼  ▼                    ▼

┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                          APPLICATION LAYER (Core Services)                               │
├──────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                           │
│  ┌────────────────────────────────────┐    ┌────────────────────────────────────┐      │
│  │      EOC-TeamsFx (React App)       │    │   EOC-Extensions (SPFX)            │      │
│  │                                    │    │                                    │      │
│  │  ┌──────────────────────────────┐ │    │  ┌──────────────────────────────┐ │      │
│  │  │ Tab Component                │ │    │  │ ListView Command Set         │ │      │
│  │  │ - Dashboard                  │ │    │  │ - Notify to Teams Group      │ │      │
│  │  │ - Incident Management        │ │    │  │ - SharePoint Integration     │ │      │
│  │  │ - Config Page                │ │    │  │ - Post to Announcements      │ │      │
│  │  └──────────────────────────────┘ │    │  └──────────────────────────────┘ │      │
│  │                                    │    │                                    │      │
│  │ Tech Stack:                        │    │ Tech Stack:                        │      │
│  │ - React + TypeScript              │    │ - TypeScript (SPFX v1.13)         │      │
│  │ - Fluent UI (react-northstar)     │    │ - Microsoft Graph API            │      │
│  │ - TeamsFx SDK                     │    │ - Gulp Build System              │      │
│  │ - Microsoft Graph Client          │    │ - SharePoint REST API            │      │
│  └────────────────────────────────────┘    └────────────────────────────────────┘      │
│                                                                                           │
└──────────────────────────────────────────────────────────────────────────────────────────┘
       │                                           │
       │                                           │
       ▼                                           ▼

┌──────────────────────────────────────────���───────────────────────────────────────────────┐
│                        MICROSOFT 365 PLATFORM LAYER                                       │
├──────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                           │
│  ┌─────────────────────┐  ┌──────────────────┐  ┌──────────────────┐                  │
│  │     Microsoft       │  │    SharePoint    │  │    Microsoft     │                  │
│  │      Teams          │  │    Online        │  │     Lists        │                  │
│  │                     │  │                  │  │                  │                  │
│  │ ┌─────────────────┐ │  │ ┌──────────────┐ │  │ ┌──────────────┐ │                  │
│  │ │ Announcements   │ │  │ │ Team Sites   │ │  │ │ Incident     │ │                  │
│  │ │ Channel         │ │  │ │ (_teoc_)     │ │  │ │ Data         │ │                  │
│  │ ├─────────────────┤ │  │ ├──────────────┤ │  │ ├──────────────┤ │                  │
│  │ │ Group Chat      │ │  │ │ Site Pages   │ │  │ │ Operations   │ │                  │
│  │ │                 │ │  │ │ Library      │ │  │ │ Records      │ │                  │
│  │ └─────────────────┘ │  │ └──────────────┘ │  │ └──────────────┘ │                  │
│  │                     │  │                  │  │                  │                  │
│  └─────────────────────┘  └──────────────────┘  └──────────────────┘                  │
│                                                                                           │
└──────────────────────────────────────────────────────────────────────────────────────────┘
       │                                           │
       │                                           │
       └─────────────────────┬─────────────────────┘
                             │
       ┌─────────────────────┴─────────────────────┐
       │                                           │
       ▼                                           ▼

┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                     AUTHENTICATION & API LAYER                                            │
├──────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                           │
│  ┌──────────────────────────────┐  ┌──────────────────────────────┐                   │
│  │    Azure AD / Microsoft       │  │   Microsoft Graph API        │                   │
│  │    Identity Platform          │  │                              │                   │
│  │                               │  │ - User Authentication        │                   │
│  │ ┌──────────────────────────┐ │  │ - Teams API Endpoints        │                   │
│  │ │ OAuth 2.0 / OpenID       │ │  │ - SharePoint API             │                   │
│  │ │ Connect                  │ │  │ - Delegated Permissions      │                   │
│  │ ├──────────────────────────┤ │  └──────────────────────────────┘                   │
│  │ │ Single Sign-On (SSO)     │ │                                                     │
│  │ │ Token Management         │ │                                                     │
│  │ └──────────────────────────┘ │                                                     │
│  └──────────────────────────────┘                                                     │
│                                                                                           │
└──────────────────────────────────────────────────────────────────────────────────────────┘
                             │
                             │
                             ▼

┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                          AZURE INFRASTRUCTURE LAYER                                       │
├──────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                           │
│  ┌──────────────────────────────────────────────────────────────────────────────────┐   │
│  │                        Azure Resource Group                                      │   │
│  │                                                                                  │   │
│  │  ┌───────────────────────────────┐  ┌────────────────────────────────────────┐ │   │
│  │  │   App Service / Web App        │  │  SimpleAuth Web Application           │ │   │
│  │  │                                │  │  (Authentication Bridge)              │ │   │
│  │  │ - Hosts React Tab Application │  │  - Token Management                  │ │   │
│  │  │ - Runtime: Node.js/Web Server │  │  - Session Management                │ │   │
│  │  │ - Auto-scaling capable        │  │  - Redirect URI Handling             │ │   │
│  │  └───────────────────────────────┘  └────────────────────────────────────────┘ │   │
│  │                                                                                  │   │
│  │  Infrastructure as Code: Azure Bicep Templates                                  │   │
│  │  ├── main.bicep (Orchestration)                                                 │   │
│  │  ├── provision.bicep (Resource Creation)                                        │   │
│  │  └── config.bicep (Configuration)                                               │   │
│  │                                                                                  │   │
│  └──────────────────────────────────────────────────────────────────────────────────┘   │
│                                                                                           │
└──────────────────────────────────────────────────────────────────────────────────────────┘
                             │
                             │ (Data & API Calls)
                             │
       ┌─────────────────────┴─────────────────────┐
       │                                           │
       ▼                                           ▼

┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                              DATA FLOW EXAMPLES                                           │
├──────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                           │
│  Scenario: Notify SharePoint Update to Teams                                            │
│                                                                                          │
│  1. User in SharePoint Team Site selects a page                                         │
│  2. Clicks "Notify to Teams Group" in command menu                                      │
│  3. EOC-Extensions (SPFX) → Microsoft Graph API                                        │
│  4. Create message in Teams Announcements channel                                       │
│  5. Update reflected in Teams app in real-time                                          │
│                                                                                          │
│  Scenario: Dashboard View in Teams Tab                                                  │
│                                                                                          │
│  1. User opens Teams EOC Tab                                                            │
│  2. React component → Teams Toolkit SDK                                                 │
│  3. Authentication via Azure AD (SSO)                                                   │
│  4. Fetch incident data from Microsoft Lists via Microsoft Graph                       │
│  5. Display real-time dashboard in React UI                                            │
│                                                                                          │
└──────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Architecture Layers

TEOC follows a **multi-layer architecture** for separation of concerns and scalability:

### Layer 1: Client Layer
**Purpose**: User interaction interfaces
- Microsoft Teams (web and desktop)
- SharePoint Team Sites
- Web browsers (cross-platform)

### Layer 2: Application Layer
**Purpose**: Core business logic and UI components
- EOC-TeamsFx (React-based Teams Tab)
- EOC-Extensions (SharePoint Framework extensions)

### Layer 3: Microsoft 365 Platform
**Purpose**: Cloud services and data management
- Microsoft Teams (messaging, notifications)
- SharePoint Online (collaboration, documents)
- Microsoft Lists (structured data)

### Layer 4: Authentication & API Layer
**Purpose**: Security and service integration
- Azure Active Directory (Azure AD)
- Microsoft Graph API
- OAuth 2.0 / OpenID Connect

### Layer 5: Azure Infrastructure
**Purpose**: Cloud hosting and resource management
- Azure App Service
- Web Applications
- Azure Bicep templates (IaC)

### Layer 6: Data Layer
**Purpose**: Persistent data storage
- Microsoft 365 Cloud Storage
- Teams data, SharePoint content, Lists

---

## Core Components

### EOC-TeamsFx (React Teams Tab Application)

**Location**: `/EOC-TeamsFx`

**Key Features**:
- Dashboard Interface for real-time incident overview
- Incident Management (create, update, track)
- Configuration Pages for app customization
- Real-time synchronization

**Technology Stack**:
- React 18+ with TypeScript
- Fluent UI (react-northstar) for Teams-consistent design
- TeamsFx SDK for Teams integration
- Microsoft Graph Client for data access
- Webpack for bundling

### EOC-Extensions (SharePoint Framework Extension)

**Location**: `/EOC-Extensions`

**Key Features**:
- Notify to Teams Group command set
- Posts SharePoint updates to Teams Announcements
- Works on sites prefixed with `_teoc_`

**Technology Stack**:
- SharePoint Framework (SPFX) v1.13
- TypeScript 3.x
- Microsoft Graph API
- Gulp 4.x for automation
- Webpack 4.x for bundling

---

## Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| UI Framework | React 18+ | Component-based UI |
| Language | TypeScript 4.x+ | Type safety |
| UI Components | Fluent UI | Teams-consistent design |
| Teams SDK | TeamsFx, Teams JS SDK | Teams integration |
| SharePoint | SPFX 1.13 | Extension framework |
| Backend | Node.js, Azure App Service | Application hosting |
| Authentication | Azure AD, OAuth 2.0 | User authentication |
| APIs | Microsoft Graph API | Microsoft 365 access |
| Infrastructure | Azure Bicep | Infrastructure as Code |
| Build Tools | Webpack, Gulp | Bundling & automation |

---

## Data Flow

### Scenario 1: Dashboard Load
1. User opens TEOC Tab in Teams
2. React app initializes and checks authentication
3. TeamsFx redirects to Azure AD if needed
4. SimpleAuth exchanges auth code for tokens
5. Tab component fetches incident data via Microsoft Graph
6. Dashboard displays real-time data

### Scenario 2: SharePoint to Teams Notification
1. User selects page in SharePoint Site Pages
2. Clicks "Notify to Teams Group" from command menu
3. EOC-Extensions captures page metadata
4. Calls Microsoft Graph API to post message
5. Teams Announcements channel receives update
6. Real-time notification sent to Teams users

### Scenario 3: Incident Creation
1. User fills form in Teams Tab Dashboard
2. Form validates input data
3. Calls Microsoft Graph to create list item
4. SharePoint Lists processes request
5. Returns new incident ID and metadata
6. Component state updates and UI refreshes

---

## Deployment Pipeline

```
╔════════════════════════════════════════════════════════════════════════════════════╗
║                                DEPLOYMENT PIPELINE                                  ║
╠════════════════════════════════════════════════════════════════════════════════════╣
║                                                                                    ║
║  Local Development                                                                 ║
║  ├── npm install                                                                   ║
║  ├── teamsfx preview --local                                                       ║
║  └── Test in Teams/SharePoint locally                                              ║
║           │                                                                        ║
║           ▼                                                                        ║
║  Azure Provisioning                                                                ║
║  ├── teamsfx account login azure                                                   ║
║  ├── teamsfx provision                                                             ║
║  └── Deploy Bicep templates to Azure                                               ║
║           │                                                                        ║
║           ▼                                                                        ║
║  Azure Deployment                                                                  ║
║  ├── teamsfx deploy                                                                ║
║  ├── Push code to App Service                                                      ║
║  └── Build & run in production                                                     ║
║           │                                                                        ║
║           ▼                                                                        ║
║  Teams Publishing                                                                  ║
║  ├── teamsfx package                                                               ║
║  ├── teamsfx publish                                                               ║
║  └── Available in Teams App Store                                                  ║
║           │                                                                        ║
║           ▼                                                                        ║
║  SharePoint Deployment                                                             ║
║  ├── Build SPFX package                                                            ║
║  ├── Upload .sppkg to App Catalog                                                  ║
║  └── Install on tenant SharePoint sites                                            ║
║                                                                                    ║
╚════════════════════════════════════════════════════════════════════════════════════╝
```

---

## Repository Structure

```
╔════════════════════════════════════════════════════════════════════════════════════╗
║                            REPOSITORY STRUCTURE MAP                                 ║
╠════════════════════════════════════════════════════════════════════════════════════╣
║                                                                                    ║
║  Microsoft_TEOC/                                                                   ║
║  ├── EOC-TeamsFx/                   ← React Teams Tab Application                 ║
║  │   ├── tabs/                      ← React Components                            ║
║  │   │   ├── src/components/        ← App, Tab, TabConfig                        ║
║  │   │   └── src/                   ← Index, CSS                                  ║
║  │   ├── templates/azure/           ← Bicep Infrastructure Templates             ║
║  │   ├── package.json               ← Dependencies                                ║
║  │   └── .env.teamsfx.dev           ← Dev Configuration                          ║
║  │                                                                                ║
║  ├── EOC-Extensions/                ← SharePoint Framework Extension              ║
║  │   ├── src/                       ← TypeScript Source Code                      ║
║  │   ├── sharepoint/                ← SPFX Configuration                         ║
║  │   ├── config/                    ← Build Configuration                        ║
║  │   ├── package.json               ← Dependencies                                ║
║  │   └── gulpfile.js                ← Build Script                                ║
║  │                                                                                ║
║  ├── Deployment/                    ← Deployment Artifacts                       ║
║  ├── Wiki/                          ← Documentation                               ║
║  ├── deploy.cmd                     ← Deployment Script                          ║
║  ├── README.md                      ← Project Overview                            ║
║  ├── SECURITY.md                    ← Security Guidelines                        ║
║  └── SUPPORT.md                     ← Support Information                        ║
║                                                                                    ║
╚════════════════════════════════════════════════════════════════════════════════════╝
```