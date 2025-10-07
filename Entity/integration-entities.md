# Module 16: Integration – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name       | Description                          | Suggested Fields                                         |
|-------------------|--------------------------------------|---------------------------------------------------------|
| IntegrationType   | Type/category of integration         | IntegrationTypeID, Name, Description, Status            |
| ExternalSystem    | External system/platform             | SystemID, Name, Type, APIEndpoint, Credentials, Status  |
| APIProvider       | API provider master                  | ProviderID, Name, ContactInfo, ServiceType, Status      |

## 2. Transaction Entities

| Entity Name       | Description                          | Suggested Fields                                         |
|-------------------|--------------------------------------|---------------------------------------------------------|
| IntegrationLog    | Integration log/record               | LogID, IntegrationTypeID, SystemID, ProviderID, Request, Response, Timestamp, Status |
| DataSync          | Data synchronization event            | SyncID, SystemID, DataType, SyncedBy, SyncTime, Status  |
| PaymentGatewayTxn | Payment gateway transaction           | TxnID, SystemID, ProviderID, Amount, Currency, Reference, TxnDate, Status |
| NotificationLog   | SMS/Email/WhatsApp notification log  | NotificationID, SystemID, ProviderID, UserID, Message, Type, SentTime, Status |
| APIUsage          | API usage tracking                   | UsageID, ProviderID, SystemID, Endpoint, RequestCount, UsagePeriod, Status |
| IntegrationError  | Error/exception in integration        | ErrorID, LogID, SystemID, ProviderID, ErrorCode, ErrorMessage, Timestamp, Status |

## 3. Relations/Dependencies

- **IntegrationLog**, **APIUsage**, **PaymentGatewayTxn**, **NotificationLog**, **IntegrationError** reference **ExternalSystem** and **APIProvider**
- **IntegrationLog**, **IntegrationError** reference **IntegrationType**
- **DataSync** references **ExternalSystem**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    INTEGRATION_TYPE ||--o{ INTEGRATION_LOG : "typed"
    INTEGRATION_TYPE ||--o{ INTEGRATION_ERROR : "typed"
    EXTERNAL_SYSTEM ||--o{ INTEGRATION_LOG : "logged"
    EXTERNAL_SYSTEM ||--o{ DATA_SYNC : "synced"
    EXTERNAL_SYSTEM ||--o{ PAYMENT_GATEWAY_TXN : "transactions"
    EXTERNAL_SYSTEM ||--o{ NOTIFICATION_LOG : "notified"
    EXTERNAL_SYSTEM ||--o{ API_USAGE : "used"
    EXTERNAL_SYSTEM ||--o{ INTEGRATION_ERROR : "error"
    API_PROVIDER ||--o{ INTEGRATION_LOG : "logged"
    API_PROVIDER ||--o{ PAYMENT_GATEWAY_TXN : "transactions"
    API_PROVIDER ||--o{ NOTIFICATION_LOG : "notified"
    API_PROVIDER ||--o{ API_USAGE : "used"
    API_PROVIDER ||--o{ INTEGRATION_ERROR : "error"

    INTEGRATION_TYPE {
        int IntegrationTypeID
        string Name
        string Description
        string Status
    }
    EXTERNAL_SYSTEM {
        int SystemID
        string Name
        string Type
        string APIEndpoint
        string Credentials
        string Status
    }
    API_PROVIDER {
        int ProviderID
        string Name
        string ContactInfo
        string ServiceType
        string Status
    }
    INTEGRATION_LOG {
        int LogID
        int IntegrationTypeID
        int SystemID
        int ProviderID
        string Request
        string Response
        date Timestamp
        string Status
    }
    DATA_SYNC {
        int SyncID
        int SystemID
        string DataType
        int SyncedBy
        date SyncTime
        string Status
    }
    PAYMENT_GATEWAY_TXN {
        int TxnID
        int SystemID
        int ProviderID
        float Amount
        string Currency
        string Reference
        date TxnDate
        string Status
    }
    NOTIFICATION_LOG {
        int NotificationID
        int SystemID
        int ProviderID
        int UserID
        string Message
        string Type
        date SentTime
        string Status
    }
    API_USAGE {
        int UsageID
        int ProviderID
        int SystemID
        string Endpoint
        int RequestCount
        string UsagePeriod
        string Status
    }
    INTEGRATION_ERROR {
        int ErrorID
        int LogID
        int SystemID
        int ProviderID
        string ErrorCode
        string ErrorMessage
        date Timestamp
        string Status
    }
```
