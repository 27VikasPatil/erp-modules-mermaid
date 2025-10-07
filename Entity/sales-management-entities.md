# Module 2: Sales Management – Entity Design (Based on Module Wise Features.txt)

## 1. Master Entities

| Entity Name       | Description                       | Suggested Fields                                                      |
|-------------------|-----------------------------------|----------------------------------------------------------------------|
| Customer          | Customer master                   | CustomerID, Name, Address, Contact, TaxDetails, Level, GeoLocation, LoginCredentials, Status |
| Level             | Customer level (End User, Dealer, Distributor, etc.) | LevelID, Name, Hierarchy, Description                                |
| Area              | GeoFencing/Area master            | AreaID, Name, Pincode, City, State, Country                          |
| Product           | Product catalog                   | ProductID, Name, Category, Description, Price, Status                |
| Warehouse         | Warehouse master                  | WarehouseID, Name, Location, ManagerID, Capacity, Status             |
| PriceGroup        | Price group/scheme                | PriceGroupID, Name, Type, ValidFrom, ValidTo, AgentID                |
| Scheme            | Scheme master                     | SchemeID, Name, Description, ValidFrom, ValidTo, Status              |
| Agent             | Sales Agents                      | AgentID, Name, AreaID, ContactInfo, Status                           |

## 2. Transaction Entities

| Entity Name       | Description                       | Suggested Fields                                                      |
|-------------------|-----------------------------------|----------------------------------------------------------------------|
| Order             | Sales order                       | OrderID, CustomerID, ProductID, Quantity, Amount, Status, OrderDate, BillTo, ShipTo, WarehouseID, PriceGroupID, SchemeID, ApprovalStatus, EWayBillNo, Terms, PaymentStatus |
| SalesReturn       | Sales return                      | ReturnID, OrderID, ProductID, Quantity, Reason, ApprovalStatus, ReturnDate, Status |
| Payment           | Payments received                 | PaymentID, CustomerID, OrderID, Amount, PaymentMode, PaymentDate, Status, OverdueDays |
| Dispatch          | Dispatches and Pickups            | DispatchID, OrderID, WarehouseID, ProductID, Quantity, DispatchDate, PickupDate, AgentID, RoutePlanID, Status |
| Appointment       | Appointment scheduling            | AppointmentID, CustomerID, AgentID, Date, Purpose, Status            |
| TaskAssignment    | Task assignments for Sales Team   | TaskID, AssignedTo, AssignedBy, Description, Priority, DueDate, Status, Type |
| CustomerFeedback  | Customer feedback/rating          | FeedbackID, CustomerID, OrderID, Rating, Comments, Date, EmployeeID  |
| Meeting           | Sales meeting                     | MeetingID, CustomerID, AgentID, Date, DiscussionPoints, Outcome, NextFollowupDate, Status |
| Quotation         | Quotation management              | QuotationID, OrderID, Amount, Discount, ApprovalStatus, SharedWith, CreatedDate |
| CreditNote        | Credit note                       | CreditNoteID, CustomerID, OrderID, Amount, Reason, Date, Status      |
| DebitNote         | Debit note                        | DebitNoteID, CustomerID, OrderID, Amount, Reason, Date, Status       |
| VisitReport       | Sales visit report                | VisitReportID, AgentID, CustomerID, Date, Location, Purpose, Outcome |
| KPIReport         | KPI and Ratio Analysis            | KPIReportID, EmployeeID, Date, KPIType, Value, Comments              |
| ECommIntegration  | E-commerce order integration      | ECommOrderID, Platform, ExternalOrderID, CustomerID, ProductID, Status, Date |

## 3. Relations/Dependencies

- **Order** references **Customer**, **Product**, **PriceGroup**, **Scheme**, **Warehouse**, **Agent**
- **SalesReturn** references **Order**, **Product**
- **Payment** references **Customer**, **Order**
- **Dispatch** references **Order**, **Warehouse**, **Product**, **Agent**
- **Appointment**, **TaskAssignment** reference **Customer**, **Agent**
- **CustomerFeedback** references **Customer**, **Order**, **Employee**
- **Meeting** references **Customer**, **Agent**
- **Quotation** references **Order**
- **CreditNote/DebitNote** reference **Customer**, **Order**
- **VisitReport** references **Agent**, **Customer**
- **KPIReport** references **Employee**
- **ECommIntegration** references **Customer**, **Product**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : "places"
    CUSTOMER ||--o{ PAYMENT : "makes"
    CUSTOMER ||--o{ APPOINTMENT : "schedules"
    CUSTOMER ||--o{ CUSTOMER_FEEDBACK : "provides"
    CUSTOMER ||--o{ CREDIT_NOTE : "receives"
    CUSTOMER ||--o{ DEBIT_NOTE : "receives"
    CUSTOMER ||--o{ VISIT_REPORT : "visited"
    LEVEL ||--o{ CUSTOMER : "assigned"
    PRODUCT ||--o{ ORDER : "ordered"
    PRODUCT ||--o{ SALES_RETURN : "returned"
    PRODUCT ||--o{ ECOMM_INTEGRATION : "linked"
    WAREHOUSE ||--o{ ORDER : "fulfilled"
    WAREHOUSE ||--o{ DISPATCH : "dispatches"
    AGENT ||--o{ ORDER : "managed by"
    AGENT ||--o{ APPOINTMENT : "assigned"
    AGENT ||--o{ TASK_ASSIGNMENT : "assigned"
    AGENT ||--o{ MEETING : "attends"
    AGENT ||--o{ VISIT_REPORT : "files"
    PRICE_GROUP ||--o{ ORDER : "applied"
    SCHEME ||--o{ ORDER : "applied"
    ORDER ||--o{ SALES_RETURN : "referenced"
    ORDER ||--o{ PAYMENT : "referenced"
    ORDER ||--o{ DISPATCH : "dispatched"
    ORDER ||--o{ QUOTATION : "quoted"
    ORDER ||--o{ CREDIT_NOTE : "credited"
    ORDER ||--o{ DEBIT_NOTE : "debited"
    ORDER ||--o{ CUSTOMER_FEEDBACK : "feedback"
    EMPLOYEE ||--o{ KPI_REPORT : "KPI"
    MEETING ||--o{ ORDER : "discussed"
    ECOMM_INTEGRATION ||--o{ ORDER : "imports"

    CUSTOMER {
        int CustomerID
        string Name
        string Address
        string Contact
        string TaxDetails
        int Level
        string GeoLocation
        string LoginCredentials
        string Status
    }
    LEVEL {
        int LevelID
        string Name
        int Hierarchy
        string Description
    }
    AREA {
        int AreaID
        string Name
        string Pincode
        string City
        string State
        string Country
    }
    PRODUCT {
        int ProductID
        string Name
        string Category
        string Description
        float Price
        string Status
    }
    WAREHOUSE {
        int WarehouseID
        string Name
        string Location
        int ManagerID
        float Capacity
        string Status
    }
    PRICE_GROUP {
        int PriceGroupID
        string Name
        string Type
        date ValidFrom
        date ValidTo
        int AgentID
    }
    SCHEME {
        int SchemeID
        string Name
        string Description
        date ValidFrom
        date ValidTo
        string Status
    }
    AGENT {
        int AgentID
        string Name
        int AreaID
        string ContactInfo
        string Status
    }
    ORDER {
        int OrderID
        int CustomerID
        int ProductID
        int Quantity
        float Amount
        string Status
        date OrderDate
        string BillTo
        string ShipTo
        int WarehouseID
        int PriceGroupID
        int SchemeID
        string ApprovalStatus
        string EWayBillNo
        string Terms
        string PaymentStatus
    }
    SALES_RETURN {
        int ReturnID
        int OrderID
        int ProductID
        int Quantity
        string Reason
        string ApprovalStatus
        date ReturnDate
        string Status
    }
    PAYMENT {
        int PaymentID
        int CustomerID
        int OrderID
        float Amount
        string PaymentMode
        date PaymentDate
        string Status
        int OverdueDays
    }
    DISPATCH {
        int DispatchID
        int OrderID
        int WarehouseID
        int ProductID
        int Quantity
        date DispatchDate
        date PickupDate
        int AgentID
        int RoutePlanID
        string Status
    }
    APPOINTMENT {
        int AppointmentID
        int CustomerID
        int AgentID
        date Date
        string Purpose
        string Status
    }
    TASK_ASSIGNMENT {
        int TaskID
        int AssignedTo
        int AssignedBy
        string Description
        string Priority
        date DueDate
        string Status
        string Type
    }
    CUSTOMER_FEEDBACK {
        int FeedbackID
        int CustomerID
        int OrderID
        int Rating
        string Comments
        date Date
        int EmployeeID
    }
    MEETING {
        int MeetingID
        int CustomerID
        int AgentID
        date Date
        string DiscussionPoints
        string Outcome
        date NextFollowupDate
        string Status
    }
    QUOTATION {
        int QuotationID
        int OrderID
        float Amount
        float Discount
        string ApprovalStatus
        string SharedWith
        date CreatedDate
    }
    CREDIT_NOTE {
        int CreditNoteID
        int CustomerID
        int OrderID
        float Amount
        string Reason
        date Date
        string Status
    }
    DEBIT_NOTE {
        int DebitNoteID
        int CustomerID
        int OrderID
        float Amount
        string Reason
        date Date
        string Status
    }
    VISIT_REPORT {
        int VisitReportID
        int AgentID
        int CustomerID
        date Date
        string Location
        string Purpose
        string Outcome
    }
    KPI_REPORT {
        int KPIReportID
        int EmployeeID
        date Date
        string KPIType
        float Value
        string Comments
    }
    ECOMM_INTEGRATION {
        int ECommOrderID
        string Platform
        string ExternalOrderID
        int CustomerID
        int ProductID
        string Status
        date Date
    }
```
