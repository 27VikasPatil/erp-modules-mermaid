# Module 14: Mobile App – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name   | Description                        | Suggested Fields                                         |
|---------------|------------------------------------|---------------------------------------------------------|
| AppUser       | Mobile app user                    | UserID, Name, Role, ContactInfo, Type, Status           |
| Device        | Mobile device master               | DeviceID, UserID, DeviceType, OS, IMEI, AppVersion, Status |
| AppModule     | Module available in app            | ModuleID, Name, Description, Status                     |

## 2. Transaction Entities

| Entity Name   | Description                        | Suggested Fields                                         |
|---------------|------------------------------------|---------------------------------------------------------|
| Login         | User login record                  | LoginID, UserID, DeviceID, LoginTime, LogoutTime, Status|
| DataEntry     | Data entry from app                | EntryID, UserID, ModuleID, DataType, Data, EntryTime, Status |
| ProgressUpdate| Field progress/status update       | UpdateID, UserID, ModuleID, DataType, Data, UpdateTime, Status |
| Notification  | App notification                   | NotificationID, UserID, Message, Type, Date, Status     |
| AppFeedback   | User feedback/report               | FeedbackID, UserID, ModuleID, Feedback, Rating, Date, Status |
| DashboardView | Dashboard view from app            | ViewID, UserID, ModuleID, ViewTime, Status              |

## 3. Relations/Dependencies

- **AppUser** is linked to **Device**, **Login**, **DataEntry**, **ProgressUpdate**, **Notification**, **AppFeedback**, **DashboardView**
- **AppModule** is referenced in **DataEntry**, **ProgressUpdate**, **DashboardView**, **AppFeedback**
- **Device** is referenced in **Login**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    APP_USER ||--o{ DEVICE : "uses"
    APP_USER ||--o{ LOGIN : "logs in"
    APP_USER ||--o{ DATA_ENTRY : "enters"
    APP_USER ||--o{ PROGRESS_UPDATE : "updates"
    APP_USER ||--o{ NOTIFICATION : "notified"
    APP_USER ||--o{ APP_FEEDBACK : "feedback"
    APP_USER ||--o{ DASHBOARD_VIEW : "views"
    DEVICE ||--o{ LOGIN : "login"
    DATA_ENTRY ||--o{ APP_MODULE : "module"
    PROGRESS_UPDATE ||--o{ APP_MODULE : "module"
    APP_FEEDBACK ||--o{ APP_MODULE : "module"
    DASHBOARD_VIEW ||--o{ APP_MODULE : "module"

    APP_USER {
        int UserID
        string Name
        string Role
        string ContactInfo
        string Type
        string Status
    }
    DEVICE {
        int DeviceID
        int UserID
        string DeviceType
        string OS
        string IMEI
        string AppVersion
        string Status
    }
    APP_MODULE {
        int ModuleID
        string Name
        string Description
        string Status
    }
    LOGIN {
        int LoginID
        int UserID
        int DeviceID
        date LoginTime
        date LogoutTime
        string Status
    }
    DATA_ENTRY {
        int EntryID
        int UserID
        int ModuleID
        string DataType
        string Data
        date EntryTime
        string Status
    }
    PROGRESS_UPDATE {
        int UpdateID
        int UserID
        int ModuleID
        string DataType
        string Data
        date UpdateTime
        string Status
    }
    NOTIFICATION {
        int NotificationID
        int UserID
        string Message
        string Type
        date Date
        string Status
    }
    APP_FEEDBACK {
        int FeedbackID
        int UserID
        int ModuleID
        string Feedback
        float Rating
        date Date
        string Status
    }
    DASHBOARD_VIEW {
        int ViewID
        int UserID
        int ModuleID
        date ViewTime
        string Status
    }
```
