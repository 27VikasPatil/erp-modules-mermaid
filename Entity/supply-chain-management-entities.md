# Module 4: Supply Chain Management – Entity Design (Based on Module Wise Features.txt)

## 1. Master Entities

| Entity Name     | Description                         | Suggested Fields                                           |
|-----------------|-------------------------------------|-----------------------------------------------------------|
| Warehouse       | Warehouse master                    | WarehouseID, Name, Location, ManagerID, Capacity, Status  |
| Vehicle         | Vehicle tracking/management         | VehicleID, Number, Type, DriverID, Capacity, Status       |
| Route           | Route plan master                   | RouteID, Name, StartLocation, EndLocation, Distance, Status|
| User            | Supply chain/logistics users        | UserID, Name, Role, ContactInfo, Status                   |
| ThirdPartyVendor| External logistics/transport vendors| VendorID, Name, ServiceType, ContactInfo, Status          |

## 2. Transaction Entities

| Entity Name     | Description                         | Suggested Fields                                           |
|-----------------|-------------------------------------|-----------------------------------------------------------|
| DistributionPlan| Distribution plan for goods         | DistPlanID, WarehouseID, ProductID, Quantity, Date, RouteID, Status |
| Dispatch        | Dispatch of goods                   | DispatchID, DistPlanID, VehicleID, DriverID, Date, Status |
| Pickup          | Pickup of goods                     | PickupID, WarehouseID, ProductID, Quantity, Date, VehicleID, DriverID, Status |
| StockMovement   | Inward/outward stock movements      | MovementID, WarehouseID, ProductID, Quantity, MovementType, Date, ReferenceID, Status |
| RouteExpense    | Driver/vehicle route expenses       | ExpenseID, RouteID, VehicleID, DriverID, Amount, Date, Type, Status |
| VehicleTracking | Vehicle tracking/integration        | TrackingID, VehicleID, Location, Timestamp, Status        |
| RiskManagement  | Risk, insurance, fines management   | RiskID, VehicleID, Type, Details, Amount, Date, Status    |
| VendorAssignment| Third party vendor assignment       | AssignmentID, VendorID, DispatchID, ServiceType, Date, Status |

## 3. Relations/Dependencies

- **DistributionPlan** references **Warehouse**, **Product**, **Route**
- **Dispatch** references **DistributionPlan**, **Vehicle**, **User** (Driver)
- **Pickup** references **Warehouse**, **Product**, **Vehicle**, **User** (Driver)
- **StockMovement** references **Warehouse**, **Product**, and can reference **Dispatch** or **Pickup**
- **RouteExpense** references **Route**, **Vehicle**, **User**
- **VehicleTracking** references **Vehicle**
- **RiskManagement** references **Vehicle**
- **VendorAssignment** references **ThirdPartyVendor**, **Dispatch**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    WAREHOUSE ||--o{ DISTRIBUTION_PLAN : "plans"
    WAREHOUSE ||--o{ PICKUP : "picks"
    WAREHOUSE ||--o{ STOCK_MOVEMENT : "moves"
    VEHICLE ||--o{ DISPATCH : "dispatched"
    VEHICLE ||--o{ PICKUP : "picked"
    VEHICLE ||--o{ ROUTE_EXPENSE : "expenses"
    VEHICLE ||--o{ VEHICLE_TRACKING : "tracked"
    VEHICLE ||--o{ RISK_MANAGEMENT : "risk"
    ROUTE ||--o{ DISTRIBUTION_PLAN : "assigned"
    ROUTE ||--o{ ROUTE_EXPENSE : "expenses"
    USER ||--o{ DISPATCH : "drives"
    USER ||--o{ PICKUP : "drives"
    USER ||--o{ ROUTE_EXPENSE : "incurred"
    THIRD_PARTY_VENDOR ||--o{ VENDOR_ASSIGNMENT : "assigned"
    DISPATCH ||--o{ VENDOR_ASSIGNMENT : "vendor"
    PRODUCT ||--o{ DISTRIBUTION_PLAN : "planned"
    PRODUCT ||--o{ PICKUP : "picked"
    PRODUCT ||--o{ STOCK_MOVEMENT : "moved"
    DISTRIBUTION_PLAN ||--o{ DISPATCH : "dispatched"
    DISTRIBUTION_PLAN ||--o{ STOCK_MOVEMENT : "reference"
    PICKUP ||--o{ STOCK_MOVEMENT : "reference"

    WAREHOUSE {
        int WarehouseID
        string Name
        string Location
        int ManagerID
        float Capacity
        string Status
    }
    VEHICLE {
        int VehicleID
        string Number
        string Type
        int DriverID
        float Capacity
        string Status
    }
    ROUTE {
        int RouteID
        string Name
        string StartLocation
        string EndLocation
        float Distance
        string Status
    }
    USER {
        int UserID
        string Name
        string Role
        string ContactInfo
        string Status
    }
    THIRD_PARTY_VENDOR {
        int VendorID
        string Name
        string ServiceType
        string ContactInfo
        string Status
    }
    DISTRIBUTION_PLAN {
        int DistPlanID
        int WarehouseID
        int ProductID
        float Quantity
        date Date
        int RouteID
        string Status
    }
    DISPATCH {
        int DispatchID
        int DistPlanID
        int VehicleID
        int DriverID
        date Date
        string Status
    }
    PICKUP {
        int PickupID
        int WarehouseID
        int ProductID
        float Quantity
        date Date
        int VehicleID
        int DriverID
        string Status
    }
    STOCK_MOVEMENT {
        int MovementID
        int WarehouseID
        int ProductID
        float Quantity
        string MovementType
        date Date
        int ReferenceID
        string Status
    }
    ROUTE_EXPENSE {
        int ExpenseID
        int RouteID
        int VehicleID
        int DriverID
        float Amount
        date Date
        string Type
        string Status
    }
    VEHICLE_TRACKING {
        int TrackingID
        int VehicleID
        string Location
        date Timestamp
        string Status
    }
    RISK_MANAGEMENT {
        int RiskID
        int VehicleID
        string Type
        string Details
        float Amount
        date Date
        string Status
    }
    VENDOR_ASSIGNMENT {
        int AssignmentID
        int VendorID
        int DispatchID
        string ServiceType
        date Date
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
```
