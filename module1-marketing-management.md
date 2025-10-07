mermaid
%% Marketing Management - Swimlane
%% Roles: Marketing, Sales, Manager

sequenceDiagram
    participant Marketing
    participant Sales
    participant Manager

    Marketing->>Marketing: Create Campaign
    Marketing->>Manager: Request Approval
    Manager->>Marketing: Approve/Reject
    Marketing->>Sales: Assign Leads
    Sales->>Sales: Follow-up, Convert Deal
    Sales->>Manager: Submit Quotation for Approval
    Manager->>Sales: Approve/Reject Quotation
    Marketing->>Marketing: Track Performance
    Manager->>Manager: Review Reports
