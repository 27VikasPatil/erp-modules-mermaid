# Module 17: Settings Management – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name     | Description                       | Suggested Fields                                         |
|-----------------|-----------------------------------|---------------------------------------------------------|
| Country         | Country master                    | CountryID, Name, Code, Status                           |
| State           | State master                      | StateID, CountryID, Name, Code, Status                  |
| City            | City master                       | CityID, StateID, Name, Pincode, Status                  |
| Unit            | Measuring Unit master             | UnitID, Name, Type, Symbol, Status                      |
| Department      | Department master                 | DeptID, Name, AreaID, ManagerID, Status                 |
| Area            | Area master                       | AreaID, Name, Pincode, CityID, Status                   |
| DocumentType    | Document type master              | DocTypeID, Name, Description, Status                    |
| CustomField     | Custom field definition           | FieldID, Name, EntityType, DataType, DefaultValue, Status|
| CompanySetting  | Company setting master            | SettingID, Name, Value, Description, Status             |
| TypeManagement  | Type management (Leave, Service, etc.) | TypeID, Name, Category, Status                        |
| FormTemplate    | Form template master              | TemplateID, Name, EntityType, Format, Status            |

## 2. Transaction Entities

| Entity Name     | Description                       | Suggested Fields                                         |
|-----------------|-----------------------------------|---------------------------------------------------------|
| SettingChange   | Change log for settings           | ChangeID, SettingID, OldValue, NewValue, ChangedBy, ChangeDate, Status |
| DocumentUpload  | Document upload record            | UploadID, DocTypeID, EntityID, FileName, FilePath, UploadedBy, Date, Status |
| CustomFieldValue| Custom field value for entity     | ValueID, FieldID, EntityID, Value, SetBy, SetDate, Status|

## 3. Relations/Dependencies

- **State** references **Country**
- **City** references **State**
- **Area** references **City**
- **Department** references **Area**
- **DocumentUpload** references **DocumentType**, any entity via **EntityID**
- **CustomFieldValue** references **CustomField**, any entity via **EntityID**
- **SettingChange** references **CompanySetting**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    COUNTRY ||--o{ STATE : "states"
    STATE ||--o{ CITY : "cities"
    CITY ||--o{ AREA : "areas"
    AREA ||--o{ DEPARTMENT : "departments"
    DEPARTMENT ||--o{ COMPANY_SETTING : "settings"
    DOCUMENT_TYPE ||--o{ DOCUMENT_UPLOAD : "uploaded"
    CUSTOM_FIELD ||--o{ CUSTOM_FIELD_VALUE : "values"
    COMPANY_SETTING ||--o{ SETTING_CHANGE : "changed"
    FORM_TEMPLATE ||--o{ DOCUMENT_TYPE : "typed"
    TYPE_MANAGEMENT ||--o{ FORM_TEMPLATE : "managed"

    COUNTRY {
        int CountryID
        string Name
        string Code
        string Status
    }
    STATE {
        int StateID
        int CountryID
        string Name
        string Code
        string Status
    }
    CITY {
        int CityID
        int StateID
        string Name
        string Pincode
        string Status
    }
    UNIT {
        int UnitID
        string Name
        string Type
        string Symbol
        string Status
    }
    DEPARTMENT {
        int DeptID
        string Name
        int AreaID
        int ManagerID
        string Status
    }
    AREA {
        int AreaID
        string Name
        string Pincode
        int CityID
        string Status
    }
    DOCUMENT_TYPE {
        int DocTypeID
        string Name
        string Description
        string Status
    }
    CUSTOM_FIELD {
        int FieldID
        string Name
        string EntityType
        string DataType
        string DefaultValue
        string Status
    }
    COMPANY_SETTING {
        int SettingID
        string Name
        string Value
        string Description
        string Status
    }
    TYPE_MANAGEMENT {
        int TypeID
        string Name
        string Category
        string Status
    }
    FORM_TEMPLATE {
        int TemplateID
        string Name
        string EntityType
        string Format
        string Status
    }
    SETTING_CHANGE {
        int ChangeID
        int SettingID
        string OldValue
        string NewValue
        int ChangedBy
        date ChangeDate
        string Status
    }
    DOCUMENT_UPLOAD {
        int UploadID
        int DocTypeID
        int EntityID
        string FileName
        string FilePath
        int UploadedBy
        date Date
        string Status
    }
    CUSTOM_FIELD_VALUE {
        int ValueID
        int FieldID
        int EntityID
        string Value
        int SetBy
        date SetDate
        string Status
    }
```
