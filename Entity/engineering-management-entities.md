# Module 6: Engineering Management – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name         | Description                             | Suggested Fields                                                  |
|---------------------|-----------------------------------------|------------------------------------------------------------------|
| ProductConfig       | Product configuration master            | ProductID, Name, ShipmentType, Code, Category, WarrantyPeriod, Status, Price, StockDetails, BatchNo, SerialNo, Unit, MinStock, MaxStock, WarehouseDetails |
| ProcessDefinition   | Manufacturing process definitions       | ProcessID, Name, Description, SOPID, Status                      |
| SOP                 | Standard Operating Procedures           | SOPID, Name, Description, Document, EffectiveDate, Status        |
| BOM                 | Bill of Material master                 | BOMID, ProductID, RawMaterialList, Quantity, Version, Status     |
| RawMaterial         | Raw material library                    | MaterialID, Name, Category, Unit, MinStock, MaxStock, Status     |
| ToolAccessory       | Tools & accessories                     | ToolID, Name, Type, Description, Status                          |
| Vehicle             | Vehicle master                          | VehicleID, Name, Type, Number, Capacity, Status                  |
| Document            | Engineering documents                   | DocumentID, Name, Type, RelatedEntityID, FilePath, Status        |

## 2. Transaction Entities

| Entity Name         | Description                             | Suggested Fields                                                  |
|---------------------|-----------------------------------------|------------------------------------------------------------------|
| ProductMaintenance  | Maintenance record for product/vehicle  | MaintenanceID, ProductID, VehicleID, MaintenanceType, Date, PerformedBy, Status, Remarks |
| SOPAssignment       | SOP/process assignment                  | AssignmentID, SOPID, AssignedTo, AssignmentDate, Status          |
| BOMRevision         | BOM revision/change history              | RevisionID, BOMID, ChangedBy, ChangeDate, ChangeDetails, Version |
| ToolAssignment      | Tool/Accessory assignment               | ToolAssignmentID, ToolID, AssignedTo, AssignmentDate, Status     |
| VehicleAssignment   | Vehicle assignment                      | VehicleAssignmentID, VehicleID, AssignedTo, AssignmentDate, RouteID, Status |
| InsurancePUC        | Insurance/PUC/Passing details           | InsuranceID, VehicleID, Type, PolicyNo, ValidFrom, ValidTo, FilePath, Status |
| RouteManagement     | Route management for vehicles            | RouteMgmtID, VehicleID, RouteID, AssignedDate, Status            |
| EngineeringAudit    | Engineering audit record                | AuditID, EntityType, EntityID, AuditDate, AuditedBy, Findings, Status |
| CalibrationRecord   | Calibration for instruments/tools        | CalibrationID, ToolID, Date, CalibratedBy, Result, Status        |

## 3. Relations/Dependencies

- **ProductConfig** links to **BOM**, **SOP**, **RawMaterial**, **WarehouseDetails**, **BatchNo**, **SerialNo**
- **ProcessDefinition** references **SOP**
- **BOM** references **ProductConfig**, **RawMaterial**
- **ToolAssignment** and **CalibrationRecord** reference **ToolAccessory**
- **VehicleAssignment**, **RouteManagement**, **InsurancePUC** reference **Vehicle**
- **Document** references any entity via **RelatedEntityID**
- **ProductMaintenance** references **ProductConfig**, **Vehicle**
- **EngineeringAudit** references all core entities

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    PRODUCT_CONFIG ||--o{ BOM : "bill of material"
    PRODUCT_CONFIG ||--o{ SOP : "SOP"
    PRODUCT_CONFIG ||--o{ PRODUCT_MAINTENANCE : "maintenance"
    PRODUCT_CONFIG ||--o{ ENGINEERING_AUDIT : "audited"
    PRODUCT_CONFIG ||--o{ DOCUMENT : "documented"
    BOM ||--o{ RAW_MATERIAL : "materials"
    BOM ||--o{ BOM_REVISION : "revision"
    SOP ||--o{ SOP_ASSIGNMENT : "assigned"
    SOP ||--o{ PROCESS_DEFINITION : "process"
    TOOL_ACCESSORY ||--o{ TOOL_ASSIGNMENT : "assigned"
    TOOL_ACCESSORY ||--o{ CALIBRATION_RECORD : "calibrated"
    VEHICLE ||--o{ VEHICLE_ASSIGNMENT : "assigned"
    VEHICLE ||--o{ INSURANCE_PUC : "insured"
    VEHICLE ||--o{ ROUTE_MANAGEMENT : "routed"
    VEHICLE ||--o{ PRODUCT_MAINTENANCE : "maintained"
    VEHICLE ||--o{ ENGINEERING_AUDIT : "audited"
    DOCUMENT ||--o{ PRODUCT_CONFIG : "product"
    DOCUMENT ||--o{ BOM : "bom"
    DOCUMENT ||--o{ SOP : "sop"
    DOCUMENT ||--o{ VEHICLE : "vehicle"
    ENGINEERING_AUDIT ||--o{ PRODUCT_CONFIG : "product"
    ENGINEERING_AUDIT ||--o{ VEHICLE : "vehicle"
    ENGINEERING_AUDIT ||--o{ TOOL_ACCESSORY : "tool"
    ENGINEERING_AUDIT ||--o{ SOP : "sop"
    ENGINEERING_AUDIT ||--o{ BOM : "bom"

    PRODUCT_CONFIG {
        int ProductID
        string Name
        string ShipmentType
        string Code
        string Category
        string WarrantyPeriod
        string Status
        float Price
        string StockDetails
        string BatchNo
        string SerialNo
        string Unit
        float MinStock
        float MaxStock
        string WarehouseDetails
    }
    BOM {
        int BOMID
        int ProductID
        string RawMaterialList
        float Quantity
        string Version
        string Status
    }
    SOP {
        int SOPID
        string Name
        string Description
        string Document
        date EffectiveDate
        string Status
    }
    PROCESS_DEFINITION {
        int ProcessID
        string Name
        string Description
        int SOPID
        string Status
    }
    RAW_MATERIAL {
        int MaterialID
        string Name
        string Category
        string Unit
        float MinStock
        float MaxStock
        string Status
    }
    TOOL_ACCESSORY {
        int ToolID
        string Name
        string Type
        string Description
        string Status
    }
    VEHICLE {
        int VehicleID
        string Name
        string Type
        string Number
        float Capacity
        string Status
    }
    DOCUMENT {
        int DocumentID
        string Name
        string Type
        int RelatedEntityID
        string FilePath
        string Status
    }
    PRODUCT_MAINTENANCE {
        int MaintenanceID
        int ProductID
        int VehicleID
        string MaintenanceType
        date Date
        int PerformedBy
        string Status
        string Remarks
    }
    SOP_ASSIGNMENT {
        int AssignmentID
        int SOPID
        int AssignedTo
        date AssignmentDate
        string Status
    }
    BOM_REVISION {
        int RevisionID
        int BOMID
        int ChangedBy
        date ChangeDate
        string ChangeDetails
        string Version
    }
    TOOL_ASSIGNMENT {
        int ToolAssignmentID
        int ToolID
        int AssignedTo
        date AssignmentDate
        string Status
    }
    VEHICLE_ASSIGNMENT {
        int VehicleAssignmentID
        int VehicleID
        int AssignedTo
        date AssignmentDate
        int RouteID
        string Status
    }
    INSURANCE_PUC {
        int InsuranceID
        int VehicleID
        string Type
        string PolicyNo
        date ValidFrom
        date ValidTo
        string FilePath
        string Status
    }
    ROUTE_MANAGEMENT {
        int RouteMgmtID
        int VehicleID
        int RouteID
        date AssignedDate
        string Status
    }
    ENGINEERING_AUDIT {
        int AuditID
        string EntityType
        int EntityID
        date AuditDate
        int AuditedBy
        string Findings
        string Status
    }
    CALIBRATION_RECORD {
        int CalibrationID
        int ToolID
        date Date
        int CalibratedBy
        string Result
        string Status
    }
```
