# Module 15: Portal – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name   | Description                        | Suggested Fields                                         |
|---------------|------------------------------------|---------------------------------------------------------|
| PortalUser    | Portal user                        | UserID, Name, Role, ContactInfo, Type, Status           |
| PortalModule  | Portal module                      | ModuleID, Name, Description, Status                     |
| PortalRole    | Portal role & permissions          | RoleID, Name, Description, Permissions, Status          |

## 2. Transaction Entities

| Entity Name   | Description                        | Suggested Fields                                         |
|---------------|------------------------------------|---------------------------------------------------------|
| Login         | Portal login record                | LoginID, UserID, LoginTime, LogoutTime, IP, Status      |
| Request       | Workflow/request from portal       | RequestID, UserID, ModuleID, RequestType, Data, RequestedDate, Status |
| Approval      | Approval workflow                  | ApprovalID, RequestID, ApprovedBy, ApprovalDate, Status |
| DocumentExchange| Document upload/download         | DocExID, UserID, ModuleID, FileName, FilePath, Action, Date, Status |
| Feedback      | User feedback                      | FeedbackID, UserID, ModuleID, Feedback, Rating, Date, Status |
| Notification  | Portal notification                | NotificationID, UserID, Message, Type, Date, Status     |
| StatusUpdate  | Request/status update              | StatusID, RequestID, Status, UpdatedBy, UpdateDate      |

## 3. Relations/Dependencies

- **PortalUser** linked to **Login**, **Request**, **Feedback**, **Notification**, **DocumentExchange**
- **PortalModule** referenced in **Request**, **Feedback**, **DocumentExchange**
- **PortalRole** assigned to **PortalUser**
- **Approval** references **Request**
- **StatusUpdate** references **Request**
- **DocumentExchange** references **PortalUser**, **PortalModule**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    PORTAL_USER ||--o{ LOGIN : "logs in"
    PORTAL_USER ||--o{ REQUEST : "requests"
    PORTAL_USER ||--o{ FEEDBACK : "feedback"
    PORTAL_USER ||--o{ NOTIFICATION : "notified"
    PORTAL_USER ||--o{ DOCUMENT_EXCHANGE : "documents"
    PORTAL_USER ||--o{ PORTAL_ROLE : "role"
    PORTAL_MODULE ||--o{ REQUEST : "module"
    PORTAL_MODULE ||--o{ FEEDBACK : "feedback"
    PORTAL_MODULE ||--o{ DOCUMENT_EXCHANGE : "documents"
    PORTAL_ROLE ||--o{ PORTAL_USER : "assigned"
    REQUEST ||--o{ APPROVAL : "approved"
    REQUEST ||--o{ STATUS_UPDATE : "status"
    DOCUMENT_EXCHANGE ||--o{ PORTAL_USER : "uploaded"
    DOCUMENT_EXCHANGE ||--o{ PORTAL_MODULE : "for module"

    PORTAL_USER {
        int UserID
        string Name
        string Role
        string ContactInfo
        string Type
        string Status
    }
    PORTAL_MODULE {
        int ModuleID
        string Name
        string Description
        string Status
    }
    PORTAL_ROLE {
        int RoleID
        string Name
        string Description
        string Permissions
        string Status
    }
    LOGIN {
        int LoginID
        int UserID
        date LoginTime
        date LogoutTime
        string IP
        string Status
    }
    REQUEST {
        int RequestID
        int UserID
        int ModuleID
        string RequestType
        string Data
        date RequestedDate
        string Status
    }
    APPROVAL {
        int ApprovalID
        int RequestID
        int ApprovedBy
        date ApprovalDate
        string Status
    }
    DOCUMENT_EXCHANGE {
        int DocExID
        int UserID
        int ModuleID
        string FileName
        string FilePath
        string Action
        date Date
        string Status
    }
    FEEDBACK {
        int FeedbackID
        int UserID
        int ModuleID
        string Feedback
        float Rating
        date Date
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
    STATUS_UPDATE {
        int StatusID
        int RequestID
        string Status
        int UpdatedBy
        date UpdateDate
    }
```
