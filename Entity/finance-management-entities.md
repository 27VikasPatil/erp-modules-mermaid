# Module 11: Finance Management – Entity Design (Based on Module Wise Features.txt SRS)

## 1. Master Entities

| Entity Name      | Description                                   | Suggested Fields                                        |
|------------------|-----------------------------------------------|--------------------------------------------------------|
| InvoiceType      | Invoice type master                           | TypeID, Name, Description, Status                      |
| Account          | Account master                                | AccountID, Name, Type, DeptID, Status                  |
| Budget           | Budget master                                 | BudgetID, DeptID, Year, Amount, ApprovedBy, Status     |
| TaxType          | Tax type master (GST, TDS, etc.)              | TaxID, Name, Rate, EffectiveDate, Status               |
| ExpenseType      | Expense type master                           | ExpenseTypeID, Name, Description, Status               |

## 2. Transaction Entities

| Entity Name      | Description                                   | Suggested Fields                                        |
|------------------|-----------------------------------------------|--------------------------------------------------------|
| Invoice          | Sales/service/purchase invoice                | InvoiceID, InvoiceTypeID, AccountID, OrderID, Amount, TaxID, Date, Status |
| Payment          | Payment receipt                                | PaymentID, InvoiceID, AccountID, Amount, PaymentMode, PaymentDate, Status |
| DebitNote        | Debit note                                    | DebitNoteID, AccountID, InvoiceID, Amount, Reason, Date, Status |
| CreditNote       | Credit note                                   | CreditNoteID, AccountID, InvoiceID, Amount, Reason, Date, Status |
| Expense          | Expense record                                | ExpenseID, AccountID, ExpenseTypeID, Amount, SubmittedBy, Date, Status, Attachment |
| BudgetApproval   | Budget approval workflow                      | ApprovalID, BudgetID, ApprovedBy, ApprovalDate, Status |
| BalanceSheet     | Balance sheet record                          | SheetID, Year, AccountID, TotalIncome, TotalExpense, Status |
| TaxCompliance    | Tax compliance record                         | ComplianceID, AccountID, TaxID, Amount, FilingDate, Status |
| CashFlow         | Cash flow management                          | CashFlowID, AccountID, Inflow, Outflow, Date, Status   |
| Forecast         | Financial forecasting                         | ForecastID, AccountID, Year, ForecastAmount, Comments, Status |
| Outstanding      | Outstanding payment tracking                  | OutstandingID, AccountID, InvoiceID, Amount, DueDate, Status |
| ExpenseReport    | Expense reporting                             | ReportID, AccountID, ExpenseID, Amount, ApprovedBy, Date, Status |

## 3. Relations/Dependencies

- **Invoice** references **InvoiceType**, **Account**, **OrderID**, **TaxType**
- **Payment** references **Invoice**, **Account**
- **DebitNote/CreditNote** reference **Account**, **Invoice**
- **Expense** references **Account**, **ExpenseType**
- **BudgetApproval** references **Budget**
- **BalanceSheet** references **Account**
- **TaxCompliance** references **Account**, **TaxType**
- **CashFlow**, **Forecast**, **Outstanding**, **ExpenseReport** reference **Account**

---

## 4. Mermaid ER Diagram

```mermaid
erDiagram
    INVOICE_TYPE ||--o{ INVOICE : "typed"
    ACCOUNT ||--o{ INVOICE : "invoiced"
    ACCOUNT ||--o{ PAYMENT : "paid"
    ACCOUNT ||--o{ DEBIT_NOTE : "debited"
    ACCOUNT ||--o{ CREDIT_NOTE : "credited"
    ACCOUNT ||--o{ EXPENSE : "expensed"
    ACCOUNT ||--o{ BALANCE_SHEET : "balances"
    ACCOUNT ||--o{ TAX_COMPLIANCE : "taxed"
    ACCOUNT ||--o{ CASH_FLOW : "cash flow"
    ACCOUNT ||--o{ FORECAST : "forecasted"
    ACCOUNT ||--o{ OUTSTANDING : "outstanding"
    ACCOUNT ||--o{ EXPENSE_REPORT : "reported"
    BUDGET ||--o{ BUDGET_APPROVAL : "approved"
    TAX_TYPE ||--o{ INVOICE : "taxed"
    TAX_TYPE ||--o{ TAX_COMPLIANCE : "complied"
    EXPENSE_TYPE ||--o{ EXPENSE : "typed"
    INVOICE ||--o{ PAYMENT : "paid"
    INVOICE ||--o{ DEBIT_NOTE : "debited"
    INVOICE ||--o{ CREDIT_NOTE : "credited"
    INVOICE ||--o{ OUTSTANDING : "outstanding"
    EXPENSE ||--o{ EXPENSE_REPORT : "reported"

    INVOICE_TYPE {
        int TypeID
        string Name
        string Description
        string Status
    }
    ACCOUNT {
        int AccountID
        string Name
        string Type
        int DeptID
        string Status
    }
    BUDGET {
        int BudgetID
        int DeptID
        int Year
        float Amount
        int ApprovedBy
        string Status
    }
    TAX_TYPE {
        int TaxID
        string Name
        float Rate
        date EffectiveDate
        string Status
    }
    EXPENSE_TYPE {
        int ExpenseTypeID
        string Name
        string Description
        string Status
    }
    INVOICE {
        int InvoiceID
        int InvoiceTypeID
        int AccountID
        int OrderID
        float Amount
        int TaxID
        date Date
        string Status
    }
    PAYMENT {
        int PaymentID
        int InvoiceID
        int AccountID
        float Amount
        string PaymentMode
        date PaymentDate
        string Status
    }
    DEBIT_NOTE {
        int DebitNoteID
        int AccountID
        int InvoiceID
        float Amount
        string Reason
        date Date
        string Status
    }
    CREDIT_NOTE {
        int CreditNoteID
        int AccountID
        int InvoiceID
        float Amount
        string Reason
        date Date
        string Status
    }
    EXPENSE {
        int ExpenseID
        int AccountID
        int ExpenseTypeID
        float Amount
        int SubmittedBy
        date Date
        string Status
        string Attachment
    }
    BUDGET_APPROVAL {
        int ApprovalID
        int BudgetID
        int ApprovedBy
        date ApprovalDate
        string Status
    }
    BALANCE_SHEET {
        int SheetID
        int Year
        int AccountID
        float TotalIncome
        float TotalExpense
        string Status
    }
    TAX_COMPLIANCE {
        int ComplianceID
        int AccountID
        int TaxID
        float Amount
        date FilingDate
        string Status
    }
    CASH_FLOW {
        int CashFlowID
        int AccountID
        float Inflow
        float Outflow
        date Date
        string Status
    }
    FORECAST {
        int ForecastID
        int AccountID
        int Year
        float ForecastAmount
        string Comments
        string Status
    }
    OUTSTANDING {
        int OutstandingID
        int AccountID
        int InvoiceID
        float Amount
        date DueDate
        string Status
    }
    EXPENSE_REPORT {
        int ReportID
        int AccountID
        int ExpenseID
        float Amount
        int ApprovedBy
        date Date
        string Status
    }
```
