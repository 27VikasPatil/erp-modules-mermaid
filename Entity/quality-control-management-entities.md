# Module 10: Quality Control Management – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name      | Description                          | Suggested Fields                                        |
|------------------|--------------------------------------|--------------------------------------------------------|
| QualityPlan      | Quality plan master                   | PlanID, Name, Description, CreatedBy, EffectiveDate, Status |
| CheckPoint       | Quality/process checkpoints           | CheckPointID, Name, Stage, Type, Description, Status   |
| Instrument       | Instruments/tools for QC              | InstrumentID, Name, Type, CalibrationStatus, Status    |
| Specification    | Quality specification master          | SpecID, Name, ProductID, Parameters, Status            |

## 2. Transaction Entities

| Entity Name      | Description                          | Suggested Fields                                        |
|------------------|--------------------------------------|--------------------------------------------------------|
| Inspection       | Inspection/test record                | InspectID, PlanID, ProductID, CheckPointID, Date, Result, InspectedBy, Status |
| QCReport         | QC report/analysis                    | ReportID, InspectID, ProductID, Parameters, Result, Date, Status |
| Rejection        | Rejection/rework analysis             | RejectionID, ProductID, Reason, Quantity, Action, Date, Status |
| Calibration      | Instrument/tool calibration           | CalibID, InstrumentID, Date, CalibratedBy, Result, Status |
| Ownership        | Material/machine ownership record     | OwnershipID, EntityType, EntityID, OwnerID, Date, Status |
| SampleAnalysis   | Sample piece analysis                 | SampleID, InspectID, ProductID, Result, Comments, Date, Status |

## 3. Relations/Dependencies

- **Inspection** references **QualityPlan**, **Product**, **CheckPoint**
- **QCReport** references **Inspection**, **Product**
- **Rejection** references **Product**
- **Calibration** references **Instrument**
- **Ownership** references any entity (material/machine)
- **SampleAnalysis** references **Inspection**, **Product**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    QUALITY_PLAN ||--o{ INSPECTION : "inspects"
    INSPECTION ||--o{ QC_REPORT : "reported"
    INSPECTION ||--o{ SAMPLE_ANALYSIS : "sample"
    PRODUCT ||--o{ INSPECTION : "inspected"
    PRODUCT ||--o{ QC_REPORT : "reported"
    PRODUCT ||--o{ REJECTION : "rejected"
    PRODUCT ||--o{ SAMPLE_ANALYSIS : "sample"
    CHECKPOINT ||--o{ INSPECTION : "checked"
    INSTRUMENT ||--o{ CALIBRATION : "calibrated"
    SPECIFICATION ||--o{ QC_REPORT : "spec"
    OWNERSHIP ||--o{ PRODUCT : "owned"
    OWNERSHIP ||--o{ INSTRUMENT : "owned"
    OWNERSHIP ||--o{ QUALITY_PLAN : "owned"

    QUALITY_PLAN {
        int PlanID
        string Name
        string Description
        int CreatedBy
        date EffectiveDate
        string Status
    }
    CHECKPOINT {
        int CheckPointID
        string Name
        string Stage
        string Type
        string Description
        string Status
    }
    INSTRUMENT {
        int InstrumentID
        string Name
        string Type
        string CalibrationStatus
        string Status
    }
    SPECIFICATION {
        int SpecID
        string Name
        int ProductID
        string Parameters
        string Status
    }
    INSPECTION {
        int InspectID
        int PlanID
        int ProductID
        int CheckPointID
        date Date
        string Result
        int InspectedBy
        string Status
    }
    QC_REPORT {
        int ReportID
        int InspectID
        int ProductID
        string Parameters
        string Result
        date Date
        string Status
    }
    REJECTION {
        int RejectionID
        int ProductID
        string Reason
        float Quantity
        string Action
        date Date
        string Status
    }
    CALIBRATION {
        int CalibID
        int InstrumentID
        date Date
        int CalibratedBy
        string Result
        string Status
    }
    OWNERSHIP {
        int OwnershipID
        string EntityType
        int EntityID
        int OwnerID
        date Date
        string Status
    }
    SAMPLE_ANALYSIS {
        int SampleID
        int InspectID
        int ProductID
        string Result
        string Comments
        date Date
        string Status
    }
```
