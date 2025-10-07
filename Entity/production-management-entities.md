# Module 9: Production Management – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name         | Description                                  | Suggested Fields                                                        |
|---------------------|----------------------------------------------|------------------------------------------------------------------------|
| ProductionLine      | Production line master                       | LineID, Name, Description, SOPID, Status                               |
| Product             | Product master                               | ProductID, Name, Category, Description, Unit, Status                   |
| BOM                 | Bill of Material master                      | BOMID, ProductID, RawMaterialList, Version, Status                     |
| Machine             | Machines for production                      | MachineID, Name, Type, Efficiency, Status                              |
| Employee            | Employee/Labor master                        | EmployeeID, Name, Role, Shift, Status                                  |
| CheckPoint          | Quality/Process checkpoint                   | CheckPointID, Name, Stage, Description, Status                         |

## 2. Transaction Entities

| Entity Name           | Description                                  | Suggested Fields                                                        |
|-----------------------|----------------------------------------------|------------------------------------------------------------------------|
| DailyProduction       | Daily production record                      | ProdID, LineID, ProductID, Quantity, MachineID, OperatorID, Date, Status|
| ProductionReport      | Production reporting                         | ReportID, ProdID, OutputQty, DefectQty, Date, Status                   |
| RejectionManagement   | Rejection management                         | RejectionID, ProdID, Reason, Quantity, ActionTaken, Date, Status       |
| InProcessRework       | In-process rejection/rework                  | ReworkID, ProdID, Stage, Description, Action, Date, Status             |
| CheckPointAnalysis    | Analysis at checkpoints                      | CPAnalysisID, ProdID, CheckPointID, Result, Date, Status               |
| MachineUtilization    | Machine utilization tracking                  | UtilID, MachineID, ProdID, UsageHours, Efficiency, Date, Status        |
| TaskAssignment        | Labor/task tracking                          | TaskID, ProdID, EmployeeID, TaskDesc, StartTime, EndTime, Status       |
| ProductionStatus      | Production status                            | StatusID, ProdID, Status, Date, Remarks                                |

## 3. Relations/Dependencies

- **DailyProduction** references **ProductionLine**, **Product**, **Machine**, **Employee**
- **ProductionReport**, **RejectionManagement**, **InProcessRework**, **CheckPointAnalysis**, **MachineUtilization**, **TaskAssignment**, **ProductionStatus** all reference **ProdID** (DailyProduction)
- **CheckPointAnalysis** references **CheckPoint**
- **TaskAssignment** references **Employee**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    PRODUCTION_LINE ||--o{ DAILY_PRODUCTION : "produced"
    PRODUCT ||--o{ DAILY_PRODUCTION : "produced"
    BOM ||--o{ PRODUCT : "product"
    MACHINE ||--o{ DAILY_PRODUCTION : "used"
    EMPLOYEE ||--o{ TASK_ASSIGNMENT : "assigned"
    DAILY_PRODUCTION ||--o{ PRODUCTION_REPORT : "reported"
    DAILY_PRODUCTION ||--o{ REJECTION_MANAGEMENT : "rejected"
    DAILY_PRODUCTION ||--o{ INPROCESS_REWORK : "reworked"
    DAILY_PRODUCTION ||--o{ CHECKPOINT_ANALYSIS : "checked"
    DAILY_PRODUCTION ||--o{ MACHINE_UTILIZATION : "utilized"
    DAILY_PRODUCTION ||--o{ TASK_ASSIGNMENT : "assigned"
    DAILY_PRODUCTION ||--o{ PRODUCTION_STATUS : "status"
    CHECKPOINT ||--o{ CHECKPOINT_ANALYSIS : "analysis"

    PRODUCTION_LINE {
        int LineID
        string Name
        string Description
        int SOPID
        string Status
    }
    PRODUCT {
        int ProductID
        string Name
        string Category
        string Description
        string Unit
        string Status
    }
    BOM {
        int BOMID
        int ProductID
        string RawMaterialList
        string Version
        string Status
    }
    MACHINE {
        int MachineID
        string Name
        string Type
        float Efficiency
        string Status
    }
    EMPLOYEE {
        int EmployeeID
        string Name
        string Role
        string Shift
        string Status
    }
    CHECKPOINT {
        int CheckPointID
        string Name
        string Stage
        string Description
        string Status
    }
    DAILY_PRODUCTION {
        int ProdID
        int LineID
        int ProductID
        float Quantity
        int MachineID
        int OperatorID
        date Date
        string Status
    }
    PRODUCTION_REPORT {
        int ReportID
        int ProdID
        float OutputQty
        float DefectQty
        date Date
        string Status
    }
    REJECTION_MANAGEMENT {
        int RejectionID
        int ProdID
        string Reason
        float Quantity
        string ActionTaken
        date Date
        string Status
    }
    INPROCESS_REWORK {
        int ReworkID
        int ProdID
        string Stage
        string Description
        string Action
        date Date
        string Status
    }
    CHECKPOINT_ANALYSIS {
        int CPAnalysisID
        int ProdID
        int CheckPointID
        string Result
        date Date
        string Status
    }
    MACHINE_UTILIZATION {
        int UtilID
        int MachineID
        int ProdID
        float UsageHours
        float Efficiency
        date Date
        string Status
    }
    TASK_ASSIGNMENT {
        int TaskID
        int ProdID
        int EmployeeID
        string TaskDesc
        date StartTime
        date EndTime
        string Status
    }
    PRODUCTION_STATUS {
        int StatusID
        int ProdID
        string Status
        date Date
        string Remarks
    }
```
