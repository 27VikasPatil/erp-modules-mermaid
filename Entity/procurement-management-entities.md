# Module 3: Procurement Management – Entity Design (Based on Module Wise Features.txt)

## 1. Master Entities

| Entity Name        | Description                             | Suggested Fields                                                       |
|--------------------|-----------------------------------------|-----------------------------------------------------------------------|
| Supplier           | Supplier database (local/import)        | SupplierID, Name, Address, Contact, TaxDetails, Category, Status      |
| Category           | Supplier category (local/import)        | CategoryID, Name, Description                                         |
| User               | Procurement users                       | UserID, Name, Role, ContactInfo, DeptID, Status                       |
| Product            | Products/Materials                      | ProductID, Name, Category, Description, Unit, MinStock, MaxStock, Status |
| Compliance         | Compliance requirements                 | ComplianceID, Name, Description                                       |
| ProcurementPlan    | Procurement plan master                 | PlanID, Name, Period, CreatedBy, Status                               |

## 2. Transaction Entities

| Entity Name        | Description                             | Suggested Fields                                                       |
|--------------------|-----------------------------------------|-----------------------------------------------------------------------|
| PurchaseIndent     | Purchase indent (requisition)           | IndentID, CreatedBy, ProductID, Quantity, DeptID, Date, Status        |
| SupplierSelection  | Supplier selection/comparison           | SelectionID, IndentID, SupplierID, ComparisonResult, ApprovedBy, Date |
| PurchaseOrder      | Purchase order                          | POID, SupplierID, IndentID, ProductID, Quantity, Amount, PODate, DeliveryDate, Status |
| GoodsReceipt       | Goods receipt/GRN                       | GRNID, POID, ProductID, Quantity, ReceivedBy, Date, Status            |
| ComplianceCheck    | Compliance check                        | CheckID, POID, ComplianceID, Result, CheckedBy, Date                  |
| Sampling           | Sampling/Quality check                  | SampleID, POID, ProductID, SampleResult, CheckedBy, Date              |
| PaymentTrend       | Purchase/payment trend                  | TrendID, SupplierID, POID, Amount, PaymentDate, PaymentStatus         |
| SupplyFollowup     | Supply follow-up/tracking               | FollowupID, POID, SupplierID, Comment, Date, Status                   |
| SupplierEvaluation | Supplier evaluation                     | EvaluationID, SupplierID, POID, Quality, Price, Punctuality, Compliance, Quantity, PaymentTerms, EvaluatedBy, Date |
| Requisition        | Requisition for material/manpower       | ReqID, DeptID, Type, Description, ProductID, Quantity, Date, Status   |
| MRP                | Material requirement planning           | MRPID, ProductID, PlannedQty, AvailableQty, RequiredQty, Date         |
| QualityAssurance   | Quality assurance for purchased items   | QAID, GRNID, ProductID, Result, CheckedBy, Date, Status               |

## 3. Relations/Dependencies

- **PurchaseIndent** references **User**, **Product**, **Dept**
- **SupplierSelection** references **PurchaseIndent**, **Supplier**
- **PurchaseOrder** references **Supplier**, **PurchaseIndent**, **Product**
- **GoodsReceipt** references **PurchaseOrder**, **Product**, **User**
- **ComplianceCheck**, **Sampling**, **QualityAssurance** reference **PurchaseOrder**, **Product**, **User**
- **PaymentTrend** references **Supplier**, **PurchaseOrder**
- **SupplyFollowup** references **PurchaseOrder**, **Supplier**
- **SupplierEvaluation** references **Supplier**, **PurchaseOrder**
- **Requisition** references **Dept**, **Product**
- **MRP** references **Product**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    SUPPLIER ||--o{ PURCHASE_ORDER : "receives"
    SUPPLIER ||--o{ SUPPLIER_EVALUATION : "evaluated"
    SUPPLIER ||--o{ PAYMENT_TREND : "paid"
    SUPPLIER ||--o{ SUPPLY_FOLLOWUP : "followed"
    CATEGORY ||--o{ SUPPLIER : "categorized"
    PRODUCT ||--o{ PURCHASE_INDENT : "indented"
    PRODUCT ||--o{ PURCHASE_ORDER : "ordered"
    PRODUCT ||--o{ GOODS_RECEIPT : "received"
    PRODUCT ||--o{ SAMPLING : "sampled"
    PRODUCT ||--o{ QUALITY_ASSURANCE : "assured"
    PROCUREMENT_PLAN ||--o{ PURCHASE_ORDER : "plans"
    USER ||--o{ PURCHASE_INDENT : "created"
    USER ||--o{ GOODS_RECEIPT : "received"
    USER ||--o{ COMPLIANCE_CHECK : "checked"
    USER ||--o{ SAMPLING : "checked"
    USER ||--o{ SUPPLIER_EVALUATION : "evaluates"
    COMPLIANCE ||--o{ COMPLIANCE_CHECK : "checked"
    PURCHASE_INDENT ||--o{ SUPPLIER_SELECTION : "selected"
    PURCHASE_INDENT ||--o{ PURCHASE_ORDER : "ordered"
    PURCHASE_ORDER ||--o{ GOODS_RECEIPT : "receives"
    PURCHASE_ORDER ||--o{ COMPLIANCE_CHECK : "compliance"
    PURCHASE_ORDER ||--o{ SAMPLING : "sampled"
    PURCHASE_ORDER ||--o{ PAYMENT_TREND : "paid"
    PURCHASE_ORDER ||--o{ SUPPLY_FOLLOWUP : "followed"
    PURCHASE_ORDER ||--o{ SUPPLIER_EVALUATION : "evaluated"
    GOODS_RECEIPT ||--o{ QUALITY_ASSURANCE : "quality"
    DEPT ||--o{ PURCHASE_INDENT : "indent"
    DEPT ||--o{ REQUISITION : "requisition"
    REQUISITION ||--o{ PRODUCT : "material"
    MRP ||--o{ PRODUCT : "planned"

    SUPPLIER {
        int SupplierID
        string Name
        string Address
        string Contact
        string TaxDetails
        int Category
        string Status
    }
    CATEGORY {
        int CategoryID
        string Name
        string Description
    }
    USER {
        int UserID
        string Name
        string Role
        string ContactInfo
        int DeptID
        string Status
    }
    PRODUCT {
        int ProductID
        string Name
        string Category
        string Description
        string Unit
        float MinStock
        float MaxStock
        string Status
    }
    COMPLIANCE {
        int ComplianceID
        string Name
        string Description
    }
    PROCUREMENT_PLAN {
        int PlanID
        string Name
        string Period
        int CreatedBy
        string Status
    }
    PURCHASE_INDENT {
        int IndentID
        int CreatedBy
        int ProductID
        float Quantity
        int DeptID
        date Date
        string Status
    }
    SUPPLIER_SELECTION {
        int SelectionID
        int IndentID
        int SupplierID
        string ComparisonResult
        int ApprovedBy
        date Date
    }
    PURCHASE_ORDER {
        int POID
        int SupplierID
        int IndentID
        int ProductID
        float Quantity
        float Amount
        date PODate
        date DeliveryDate
        string Status
    }
    GOODS_RECEIPT {
        int GRNID
        int POID
        int ProductID
        float Quantity
        int ReceivedBy
        date Date
        string Status
    }
    COMPLIANCE_CHECK {
        int CheckID
        int POID
        int ComplianceID
        string Result
        int CheckedBy
        date Date
    }
    SAMPLING {
        int SampleID
        int POID
        int ProductID
        string SampleResult
        int CheckedBy
        date Date
    }
    PAYMENT_TREND {
        int TrendID
        int SupplierID
        int POID
        float Amount
        date PaymentDate
        string PaymentStatus
    }
    SUPPLY_FOLLOWUP {
        int FollowupID
        int POID
        int SupplierID
        string Comment
        date Date
        string Status
    }
    SUPPLIER_EVALUATION {
        int EvaluationID
        int SupplierID
        int POID
        string Quality
        float Price
        string Punctuality
        string Compliance
        float Quantity
        string PaymentTerms
        int EvaluatedBy
        date Date
    }
    REQUISITION {
        int ReqID
        int DeptID
        string Type
        string Description
        int ProductID
        float Quantity
        date Date
        string Status
    }
    MRP {
        int MRPID
        int ProductID
        float PlannedQty
        float AvailableQty
        float RequiredQty
        date Date
    }
    QUALITY_ASSURANCE {
        int QAID
        int GRNID
        int ProductID
        string Result
        int CheckedBy
        date Date
        string Status
    }
    DEPT {
        int DeptID
        string Name
        string Description
    }
```
