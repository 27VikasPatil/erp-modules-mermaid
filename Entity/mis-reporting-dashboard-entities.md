# Module 13: MIS Reporting & Dashboard – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name   | Description                    | Suggested Fields                                         |
|---------------|-------------------------------|---------------------------------------------------------|
| ReportType    | Type/category of report        | ReportTypeID, Name, Description, Module, Status         |
| Dashboard     | Dashboard configuration        | DashboardID, Name, UserID, Module, Layout, Status       |
| KPI           | Key Performance Indicator      | KPIID, Name, Description, Module, Formula, Status       |
| Filter        | Report filter master           | FilterID, Name, Field, Operator, ValueType, Status      |

## 2. Transaction Entities

| Entity Name   | Description                    | Suggested Fields                                         |
|---------------|-------------------------------|---------------------------------------------------------|
| Report        | MIS report record              | ReportID, ReportTypeID, GeneratedBy, Date, Exported, Filters, Status |
| DashboardView | User dashboard view            | ViewID, DashboardID, UserID, ViewedDate, Status         |
| KPITracking   | KPI tracking/log               | TrackingID, KPIID, Value, Date, UserID, Status          |
| Alert         | Business alert/notification    | AlertID, ReportID, UserID, Message, Date, Status        |
| ExportLog     | Report export log              | ExportID, ReportID, ExportedBy, ExportDate, Format, Status |

## 3. Relations/Dependencies

- **Report** references **ReportType**, **User** (GeneratedBy), **Filters**
- **DashboardView** references **Dashboard**, **User**
- **KPITracking** references **KPI**, **User**
- **Alert** references **Report**, **User**
- **ExportLog** references **Report**, **User**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    REPORT_TYPE ||--o{ REPORT : "typed"
    DASHBOARD ||--o{ DASHBOARD_VIEW : "viewed"
    KPI ||--o{ KPI_TRACKING : "tracked"
    FILTER ||--o{ REPORT : "filtered"
    REPORT ||--o{ ALERT : "alerted"
    REPORT ||--o{ EXPORT_LOG : "exported"
    DASHBOARD ||--o{ USER : "owner"
    KPI ||--o{ REPORT : "reference"
    USER ||--o{ DASHBOARD_VIEW : "views"
    USER ||--o{ KPI_TRACKING : "tracks"
    USER ||--o{ REPORT : "generates"
    USER ||--o{ ALERT : "alerted"
    USER ||--o{ EXPORT_LOG : "exports"

    REPORT_TYPE {
        int ReportTypeID
        string Name
        string Description
        string Module
        string Status
    }
    DASHBOARD {
        int DashboardID
        string Name
        int UserID
        string Module
        string Layout
        string Status
    }
    KPI {
        int KPIID
        string Name
        string Description
        string Module
        string Formula
        string Status
    }
    FILTER {
        int FilterID
        string Name
        string Field
        string Operator
        string ValueType
        string Status
    }
    REPORT {
        int ReportID
        int ReportTypeID
        int GeneratedBy
        date Date
        bool Exported
        string Filters
        string Status
    }
    DASHBOARD_VIEW {
        int ViewID
        int DashboardID
        int UserID
        date ViewedDate
        string Status
    }
    KPI_TRACKING {
        int TrackingID
        int KPIID
        float Value
        date Date
        int UserID
        string Status
    }
    ALERT {
        int AlertID
        int ReportID
        int UserID
        string Message
        date Date
        string Status
    }
    EXPORT_LOG {
        int ExportID
        int ReportID
        int ExportedBy
        date ExportDate
        string Format
        string Status
    }
    USER {
        int UserID
        string Name
        string Role
        string Status
    }
```
