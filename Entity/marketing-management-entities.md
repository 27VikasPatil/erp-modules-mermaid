# Module 1: Marketing Management – Entity Design (Based on Module Wise Features.txt)

## 1. Master Entities

| Entity Name         | Description                             | Suggested Fields                                                  |
|---------------------|-----------------------------------------|------------------------------------------------------------------|
| Campaign            | Marketing campaigns                      | CampaignID, Name, Type, StartDate, EndDate, Budget, Status, Owner, Channel, TemplateID, PerformanceTarget |
| Template            | SMS/Email/WhatsApp templates             | TemplateID, Name, Content, Channel, Active, CreatedBy            |
| Event               | Upcoming events                          | EventID, Name, Type, Date, Location, Status, RelatedCampaignID    |
| LeadSource          | Lead source (Online/Offline/3rd Party)   | LeadSourceID, Name, Channel, IntegrationType, Active              |
| ProductService      | Products/services offered                | ProductServiceID, Name, Category, Description                     |
| Segment             | Target customer segment                  | SegmentID, Name, Criteria, Description                            |
| Agent               | Agents/ISD/Technicians/Workers           | AgentID, Name, Role, ContactInfo, Active                          |
| PerformanceTarget   | Budget/Targets/ABP/Employee/Partner      | TargetID, Type, Value, Period, OwnerID, Department, Status        |
| PriceList           | Price list/scheme/offer management       | PriceListID, Name, ValidFrom, ValidTo, OwnerID, Status            |
| Competitor          | Competitor’s data                        | CompetitorID, Name, ProductFocus, MarketShare, Comments           |

## 2. Transaction Entities

| Entity Name         | Description                             | Suggested Fields                                                  |
|---------------------|-----------------------------------------|------------------------------------------------------------------|
| Lead                | Individual lead record                   | LeadID, Name, ContactInfo, SourceID, CampaignID, Status, Score, AssignedTo, CreatedDate, Channel, IntegrationID |
| LeadAssignment      | Lead assignment/reassignment/ageing      | AssignmentID, LeadID, AssignedTo, AssignedDate, ReassignedDate, AgeDays, Status |
| LeadScore           | Lead scoring/evaluation                  | ScoreID, LeadID, Score, Criteria, EvaluatedBy, EvaluatedDate     |
| Deal                | Lead to deal conversion                  | DealID, LeadID, OwnerID, Stage, FollowUpDate, NegotiationStatus, PricingApproval, QuotationID, CreatedDate |
| Quotation           | Quotation management                     | QuotationID, DealID, OwnerID, PriceListID, Amount, Discount, ApprovalStatus, SharedWith, CreatedDate |
| CampaignActivity    | SMS/Email/WhatsApp/Followup              | ActivityID, CampaignID, Type, Channel, Date, Outcome, OwnerID, TemplateID |
| PerformanceTracking | Track campaign activity/performance      | PerfTrackID, CampaignID, MetricType, MetricValue, Date, OwnerID  |
| EventActivity       | Event actions (attendance, follow-up)    | EventActivityID, EventID, LeadID, Type, Outcome, OwnerID, Date   |
| CompetitorReport    | Competitor feedback/market info          | ReportID, CompetitorID, ProductServiceID, Feedback, Date, OwnerID|
| VisitReport         | Travel/visit plan and actual report      | VisitReportID, AgentID, Date, Location, Purpose, Outcome, RelatedLeadID |

## 3. Relations/Dependencies

- **Lead** references **Campaign**, **LeadSource**, **Agent**, **Channel**, and can link to **Event**
- **LeadAssignment** references **Lead** and **Agent**
- **LeadScore** references **Lead**
- **Deal** references **Lead**, **Agent**, **Quotation**
- **Quotation** references **Deal**, **PriceList**, **Owner**
- **CampaignActivity** references **Campaign**, **Template**, **Agent**
- **PerformanceTracking** references **Campaign**
- **EventActivity** references **Event**, **Lead**, **Agent**
- **CompetitorReport** references **Competitor**, **ProductService**
- **VisitReport** references **Agent**, **Lead**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    CAMPAIGN ||--o{ LEAD : "generates"
    CAMPAIGN ||--o{ CAMPAIGN_ACTIVITY : "includes"
    CAMPAIGN ||--o{ PERFORMANCE_TRACKING : "measured by"
    CAMPAIGN ||--o{ EVENT : "has event"
    CAMPAIGN ||--o{ TEMPLATE : "uses"
    LEAD_SOURCE ||--o{ LEAD : "originates"
    AGENT ||--o{ LEAD_ASSIGNMENT : "assigned to"
    AGENT ||--o{ DEAL : "owns"
    AGENT ||--o{ CAMPAIGN_ACTIVITY : "executes"
    AGENT ||--o{ EVENT_ACTIVITY : "executes"
    AGENT ||--o{ VISIT_REPORT : "visits"
    LEAD ||--o{ LEAD_ASSIGNMENT : "assignment"
    LEAD ||--o{ LEAD_SCORE : "scoring"
    LEAD ||--o{ DEAL : "converted to deal"
    LEAD ||--o{ EVENT_ACTIVITY : "event action"
    DEAL ||--o{ QUOTATION : "quotation"
    PRICE_LIST ||--o{ QUOTATION : "uses"
    TEMPLATE ||--o{ CAMPAIGN_ACTIVITY : "template"
    COMPETITOR ||--o{ COMPETITOR_REPORT : "report"
    PRODUCT_SERVICE ||--o{ COMPETITOR_REPORT : "product"
    PERFORMANCE_TARGET ||--o{ CAMPAIGN : "targets"
    CAMPAIGN ||--o{ PRICE_LIST : "offers"
    EVENT ||--o{ EVENT_ACTIVITY : "activity"
    LEAD ||--o{ VISIT_REPORT : "visit"

    CAMPAIGN {
        int CampaignID
        string Name
        string Type
        date StartDate
        date EndDate
        float Budget
        string Status
        int Owner
        string Channel
        int TemplateID
        int PerformanceTarget
    }
    TEMPLATE {
        int TemplateID
        string Name
        string Content
        string Channel
        bool Active
        int CreatedBy
    }
    EVENT {
        int EventID
        string Name
        string Type
        date Date
        string Location
        string Status
        int RelatedCampaignID
    }
    LEAD_SOURCE {
        int LeadSourceID
        string Name
        string Channel
        string IntegrationType
        bool Active
    }
    PRODUCT_SERVICE {
        int ProductServiceID
        string Name
        string Category
        string Description
    }
    SEGMENT {
        int SegmentID
        string Name
        string Criteria
        string Description
    }
    AGENT {
        int AgentID
        string Name
        string Role
        string ContactInfo
        bool Active
    }
    PERFORMANCE_TARGET {
        int TargetID
        string Type
        float Value
        string Period
        int OwnerID
        string Department
        string Status
    }
    PRICE_LIST {
        int PriceListID
        string Name
        date ValidFrom
        date ValidTo
        int OwnerID
        string Status
    }
    COMPETITOR {
        int CompetitorID
        string Name
        string ProductFocus
        string MarketShare
        string Comments
    }
    LEAD {
        int LeadID
        string Name
        string ContactInfo
        int SourceID
        int CampaignID
        string Status
        int Score
        int AssignedTo
        date CreatedDate
        string Channel
        int IntegrationID
    }
    LEAD_ASSIGNMENT {
        int AssignmentID
        int LeadID
        int AssignedTo
        date AssignedDate
        date ReassignedDate
        int AgeDays
        string Status
    }
    LEAD_SCORE {
        int ScoreID
        int LeadID
        int Score
        string Criteria
        int EvaluatedBy
        date EvaluatedDate
    }
    DEAL {
        int DealID
        int LeadID
        int OwnerID
        string Stage
        date FollowUpDate
        string NegotiationStatus
        string PricingApproval
        int QuotationID
        date CreatedDate
    }
    QUOTATION {
        int QuotationID
        int DealID
        int OwnerID
        int PriceListID
        float Amount
        float Discount
        string ApprovalStatus
        string SharedWith
        date CreatedDate
    }
    CAMPAIGN_ACTIVITY {
        int ActivityID
        int CampaignID
        string Type
        string Channel
        date Date
        string Outcome
        int OwnerID
        int TemplateID
    }
    PERFORMANCE_TRACKING {
        int PerfTrackID
        int CampaignID
        string MetricType
        float MetricValue
        date Date
        int OwnerID
    }
    EVENT_ACTIVITY {
        int EventActivityID
        int EventID
        int LeadID
        string Type
        string Outcome
        int OwnerID
        date Date
    }
    COMPETITOR_REPORT {
        int ReportID
        int CompetitorID
        int ProductServiceID
        string Feedback
        date Date
        int OwnerID
    }
    VISIT_REPORT {
        int VisitReportID
        int AgentID
        date Date
        string Location
        string Purpose
        string Outcome
        int RelatedLeadID
    }
```
