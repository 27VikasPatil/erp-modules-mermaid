# Module 7: Inventory Management – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name   | Description                                  | Suggested Fields                                                        |
|---------------|----------------------------------------------|------------------------------------------------------------------------|
| ItemCatalog   | Item definition/catalog                      | ItemID, Name, ShipmentType, Code, Type, Category, WarrantyPeriod, Status, Price, Unit, MinStock, MaxStock, WarehouseID, BatchNo, SerialNo |
| Warehouse     | Warehouse master                             | WarehouseID, Name, Location, ManagerID, Capacity, Status                |
| Customer      | Customer details (for returns/warranty)      | CustomerID, Name, Address, Contact, Status                              |

## 2. Transaction Entities

| Entity Name       | Description                                  | Suggested Fields                                                      |
|-------------------|----------------------------------------------|----------------------------------------------------------------------|
| GoodsReceipt      | Goods receipt/GRN                            | GRNID, WarehouseID, ItemID, Quantity, SourceType, SourceID, Date, Status |
| ItemIssue         | Item issue (sale/service/adjustment)         | IssueID, WarehouseID, ItemID, Quantity, IssueType, RelatedID, Date, Status |
| MaterialReturn    | Material return (sale/service/adjustment)    | ReturnID, WarehouseID, ItemID, Quantity, ReturnType, RelatedID, Date, Status |
| StockAdjustment   | Adjustment (damage, others)                  | AdjustmentID, WarehouseID, ItemID, Quantity, Reason, Date, Status      |
| StockTransfer     | Stock transfer (between warehouses/dept)     | TransferID, FromWarehouseID, ToWarehouseID, ItemID, Quantity, Date, Status |
| StockReconciliation| Stock reconciliation                        | ReconID, WarehouseID, ItemID, PhysicalQty, SystemQty, Date, Status    |
| DamageMaterial    | Damage material recording                    | DamageID, WarehouseID, ItemID, Quantity, Reason, Date, Status         |
| SerialBatchTracking| Serial/batch tracking                       | TrackID, ItemID, BatchNo, SerialNo, MovementType, Date, Status        |

## 3. Relations/Dependencies

- **ItemIssue**, **GoodsReceipt**, **MaterialReturn**, **StockAdjustment**, **StockTransfer**, **StockReconciliation**, **DamageMaterial** all reference **ItemCatalog** and **Warehouse**
- **GoodsReceipt**, **MaterialReturn** reference **SourceType** (e.g., Sale, Service, Adjustment) and **SourceID** (sale/service/other transaction)
- **SerialBatchTracking** references **ItemCatalog**, batch/serial info, and is linked to movements

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    ITEM_CATALOG ||--o{ GOODS_RECEIPT : "received"
    ITEM_CATALOG ||--o{ ITEM_ISSUE : "issued"
    ITEM_CATALOG ||--o{ MATERIAL_RETURN : "returned"
    ITEM_CATALOG ||--o{ STOCK_ADJUSTMENT : "adjusted"
    ITEM_CATALOG ||--o{ STOCK_TRANSFER : "transferred"
    ITEM_CATALOG ||--o{ STOCK_RECONCILIATION : "reconciled"
    ITEM_CATALOG ||--o{ DAMAGE_MATERIAL : "damaged"
    ITEM_CATALOG ||--o{ SERIAL_BATCH_TRACKING : "tracked"
    WAREHOUSE ||--o{ GOODS_RECEIPT : "receives"
    WAREHOUSE ||--o{ ITEM_ISSUE : "issues"
    WAREHOUSE ||--o{ MATERIAL_RETURN : "returns"
    WAREHOUSE ||--o{ STOCK_ADJUSTMENT : "adjusts"
    WAREHOUSE ||--o{ STOCK_TRANSFER : "transfers"
    WAREHOUSE ||--o{ STOCK_RECONCILIATION : "reconciles"
    WAREHOUSE ||--o{ DAMAGE_MATERIAL : "damages"
    WAREHOUSE ||--o{ SERIAL_BATCH_TRACKING : "tracks"
    GOODS_RECEIPT ||--o{ SERIAL_BATCH_TRACKING : "tracks"
    ITEM_ISSUE ||--o{ SERIAL_BATCH_TRACKING : "tracks"
    MATERIAL_RETURN ||--o{ SERIAL_BATCH_TRACKING : "tracks"
    STOCK_TRANSFER ||--o{ SERIAL_BATCH_TRACKING : "tracks"

    ITEM_CATALOG {
        int ItemID
        string Name
        string ShipmentType
        string Code
        string Type
        string Category
        string WarrantyPeriod
        string Status
        float Price
        string Unit
        float MinStock
        float MaxStock
        int WarehouseID
        string BatchNo
        string SerialNo
    }
    WAREHOUSE {
        int WarehouseID
        string Name
        string Location
        int ManagerID
        float Capacity
        string Status
    }
    CUSTOMER {
        int CustomerID
        string Name
        string Address
        string Contact
        string Status
    }
    GOODS_RECEIPT {
        int GRNID
        int WarehouseID
        int ItemID
        float Quantity
        string SourceType
        int SourceID
        date Date
        string Status
    }
    ITEM_ISSUE {
        int IssueID
        int WarehouseID
        int ItemID
        float Quantity
        string IssueType
        int RelatedID
        date Date
        string Status
    }
    MATERIAL_RETURN {
        int ReturnID
        int WarehouseID
        int ItemID
        float Quantity
        string ReturnType
        int RelatedID
        date Date
        string Status
    }
    STOCK_ADJUSTMENT {
        int AdjustmentID
        int WarehouseID
        int ItemID
        float Quantity
        string Reason
        date Date
        string Status
    }
    STOCK_TRANSFER {
        int TransferID
        int FromWarehouseID
        int ToWarehouseID
        int ItemID
        float Quantity
        date Date
        string Status
    }
    STOCK_RECONCILIATION {
        int ReconID
        int WarehouseID
        int ItemID
        float PhysicalQty
        float SystemQty
        date Date
        string Status
    }
    DAMAGE_MATERIAL {
        int DamageID
        int WarehouseID
        int ItemID
        float Quantity
        string Reason
        date Date
        string Status
    }
    SERIAL_BATCH_TRACKING {
        int TrackID
        int ItemID
        string BatchNo
        string SerialNo
        string MovementType
        date Date
        string Status
    }
```
