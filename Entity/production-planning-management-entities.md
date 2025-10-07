# Module 8: Production Planning Management – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name         | Description                                  | Suggested Fields                                                        |
|---------------------|----------------------------------------------|------------------------------------------------------------------------|
| ProductionPlan      | Production plan master                       | PlanID, Name, Period, Status, CreatedBy, CalendarType                  |
| Product             | Product/FG master                            | ProductID, Name, Category, Description, Unit, StockStatus              |
| BOM                 | Bill of Material                             | BOMID, ProductID, RawMaterialList, Version, Status                     |
| JobType             | Job type (Assembly/Manual/Calendar)          | JobTypeID, Name, Description, Status                                   |
| SOP                 | SOP master                                   | SOPID, Name, Description, Document, Status                             |
| Department          | Department master                            | DeptID, Name, ManagerID, Status                                        |

## 2. Transaction Entities

| Entity Name           | Description                                  | Suggested Fields                                                        |
|-----------------------|----------------------------------------------|------------------------------------------------------------------------|
| JobAssignment         | Job assignments (delegation to users)        | AssignmentID, PlanID, ProductID, DeptID, JobTypeID, AssignedTo, AssignmentDate, Status |
| MaterialRequest       | Material request for production              | RequestID, PlanID, ProductID, Quantity, DeptID, RequestedBy, Date, Status |
| MaterialReceivedNote  | Received material notes                      | MRNID, RequestID, ProductID, Quantity, ReceivedBy, Date, Status         |
| MaterialReturn        | Material return management                   | ReturnID, PlanID, ProductID, Quantity, DeptID, ReturnedBy, Date, Status |
| CapacityPlanning      | Capacity planning for production             | CapacityID, PlanID, ProductID, DeptID, CapacityValue, Date, Status      |
| TargetAssignment      | Target assignments for jobs                  | TargetID, PlanID, ProductID, DeptID, AssignedTo, TargetValue, Date, Status |
| AssemblyPlan          | Assembly plan/steps generation               | AssemblyID, PlanID, ProductID, BOMID, Steps, AssignedTo, Date, Status   |
| CalendarPlanning      | Planning by calendar                         | CalendarID, PlanID, ProductID, Period, AssignedTo, Status               |

## 3. Relations/Dependencies

- **ProductionPlan** links to **Product**, **BOM**, **Department**, **JobType**, **SOP**
- **JobAssignment**, **MaterialRequest**, **TargetAssignment** reference **ProductionPlan**, **Product**, **Dept**
- **MaterialReceivedNote** and **MaterialReturn** reference **MaterialRequest**, **Product**, **Dept**
- **AssemblyPlan** references **ProductionPlan**, **Product**, **BOM**
- **CapacityPlanning**, **CalendarPlanning** reference **ProductionPlan**, **Product**, **Dept**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    PRODUCTION_PLAN ||--o{ JOB_ASSIGNMENT : "assigned"
    PRODUCTION_PLAN ||--o{ MATERIAL_REQUEST : "requested"
    PRODUCTION_PLAN ||--o{ TARGET_ASSIGNMENT : "targeted"
    PRODUCTION_PLAN ||--o{ ASSEMBLY_PLAN : "assembly"
    PRODUCTION_PLAN ||--o{ CAPACITY_PLANNING : "capacity"
    PRODUCTION_PLAN ||--o{ CALENDAR_PLANNING : "calendar"
    PRODUCT ||--o{ PRODUCTION_PLAN : "planned"
    PRODUCT ||--o{ JOB_ASSIGNMENT : "assigned"
    PRODUCT ||--o{ MATERIAL_REQUEST : "requested"
    PRODUCT ||--o{ MATERIAL_RECEIVED_NOTE : "received"
    PRODUCT ||--o{ MATERIAL_RETURN : "returned"
    PRODUCT ||--o{ CAPACITY_PLANNING : "capacity"
    PRODUCT ||--o{ TARGET_ASSIGNMENT : "targeted"
    PRODUCT ||--o{ ASSEMBLY_PLAN : "assembly"
    DEPARTMENT ||--o{ PRODUCTION_PLAN : "plans"
    DEPARTMENT ||--o{ JOB_ASSIGNMENT : "assigned"
    DEPARTMENT ||--o{ MATERIAL_REQUEST : "requested"
    DEPARTMENT ||--o{ MATERIAL_RECEIVED_NOTE : "received"
    DEPARTMENT ||--o{ MATERIAL_RETURN : "returned"
    DEPARTMENT ||--o{ CAPACITY_PLANNING : "capacity"
    DEPARTMENT ||--o{ TARGET_ASSIGNMENT : "targeted"
    DEPARTMENT ||--o{ CALENDAR_PLANNING : "calendar"
    BOM ||--o{ ASSEMBLY_PLAN : "assembly"
    JOB_TYPE ||--o{ JOB_ASSIGNMENT : "job type"
    SOP ||--o{ PRODUCTION_PLAN : "sop"
    MATERIAL_REQUEST ||--o{ MATERIAL_RECEIVED_NOTE : "note"
    MATERIAL_REQUEST ||--o{ MATERIAL_RETURN : "return"

    PRODUCTION_PLAN {
        int PlanID
        string Name
        string Period
        string Status
        int CreatedBy
        string CalendarType
    }
    PRODUCT {
        int ProductID
        string Name
        string Category
        string Description
        string Unit
        string StockStatus
    }
    BOM {
        int BOMID
        int ProductID
        string RawMaterialList
        string Version
        string Status
    }
    JOB_TYPE {
        int JobTypeID
        string Name
        string Description
        string Status
    }
    SOP {
        int SOPID
        string Name
        string Description
        string Document
        string Status
    }
    DEPARTMENT {
        int DeptID
        string Name
        int ManagerID
        string Status
    }
    JOB_ASSIGNMENT {
        int AssignmentID
        int PlanID
        int ProductID
        int DeptID
        int JobTypeID
        int AssignedTo
        date AssignmentDate
        string Status
    }
    MATERIAL_REQUEST {
        int RequestID
        int PlanID
        int ProductID
        float Quantity
        int DeptID
        int RequestedBy
        date Date
        string Status
    }
    MATERIAL_RECEIVED_NOTE {
        int MRNID
        int RequestID
        int ProductID
        float Quantity
        int ReceivedBy
        date Date
        string Status
    }
    MATERIAL_RETURN {
        int ReturnID
        int PlanID
        int ProductID
        float Quantity
        int DeptID
        int ReturnedBy
        date Date
        string Status
    }
    CAPACITY_PLANNING {
        int CapacityID
        int PlanID
        int ProductID
        int DeptID
        float CapacityValue
        date Date
        string Status
    }
    TARGET_ASSIGNMENT {
        int TargetID
        int PlanID
        int ProductID
        int DeptID
        int AssignedTo
        float TargetValue
        date Date
        string Status
    }
    ASSEMBLY_PLAN {
        int AssemblyID
        int PlanID
        int ProductID
        int BOMID
        string Steps
        int AssignedTo
        date Date
        string Status
    }
    CALENDAR_PLANNING {
        int CalendarID
        int PlanID
        int ProductID
        string Period
        int AssignedTo
        string Status
    }
```
