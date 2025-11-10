# 💼 Salesforce Developer Guided Project — Expense Tracker


## 📖 Story & Context


Welcome to **BlueWave Consulting**, a medium-sized consulting firm that works with several clients. Employees often travel or buy materials for their projects, and today they report their expenses manually through email and spreadsheets.


The finance department wants to replace this process with a simple internal app in Salesforce where employees can **log expenses**, and managers can **review and approve them**.


Your goal as a Salesforce Developer is to design and implement this small app using:
- **Custom Objects** for data modeling
- **Apex** for business logic
- **Lightning Web Components (LWC)** for the modern UI
- **Visualforce** for a legacy-style report page
- **Quick Actions & Flows** for simple automation


By the end, you’ll have a working mini app that covers the full stack of Salesforce development.


---


## 🧩 Business Requirements


1. **Employees** should be able to create new expenses easily.
2. **Managers** should see a list of recent expenses and open each record.
3. **Finance** needs a simple report listing all expenses (legacy compatibility via Visualforce).
4. **The system** should allow managers to approve expenses quickly.


---


## 🧱 Step 1 — Data Model


### Create Custom Object: `Expense__c`


| Field Label | API Name | Type | Notes |
|--------------|-----------|------|-------|
| Date | Date__c | Date | When the expense was made |
| Amount | Amount__c | Currency(16,2) | Expense amount |
| Category | Category__c | Picklist | Values: Travel, Meals, Office, Other |
| Status | Status__c | Picklist | Values: Submitted, Approved, Rejected |
| Description | Description__c | Long Text Area | Optional notes |
| Employee | Employee__c | Lookup(User) | Links to the employee |


✅ **Checklist:**
- [ ] Object created and deployed
- [ ] Fields added with correct types
- [ ] Tab visible in the app


---


## ⚙️ Step 2 — Apex Controller (Logic Layer)


File: `ExpenseController.cls`


```apex
public with sharing class ExpenseController {


// TODO: Create a method to insert a new Expense__c record
@AuraEnabled
public static Expense__c createExpense(Expense__c exp) {
// Example: insert the record, return it
// TODO: Add validation and field-level security checks
insert exp;
return exp;
}


// TODO: Create a method to return a list of recent expenses
@AuraEnabled(cacheable=true)
public static List<Expense__c> getRecentExpenses() {
// Example SOQL query — refine to include current user only later
return [SELECT Id, Name, Amount__c, Category__c, Status__c, Date__c
FROM Expense__c
ORDER BY CreatedDate DESC
LIMIT 10];
}


// TODO: Add optional method for approving expenses
@AuraEnabled
public static void approveExpense(Id expenseId) {
Expense__c exp = [SELECT Id, Status__c FROM Expense__c WHERE Id = :expenseId];
exp.Status__c = 'Approved';
update exp;
}
}
```


✅ **Checklist:**
- [ ] Class compiles with no errors
- [ ] Logic methods follow best practices
- [ ] Controller tested with anonymous Apex


---


## ⚡ Step 3 — LWC: `expenseCreator`


Purpose: Allow users to enter and submit a new expense.


**Folder:** `force-app/main/default/lwc/expenseCreator`


### `expenseCreator.html`
```html
<template>
<lightning-card title="New Expense" icon-name="custom:custom63">
<div class="slds-p-around_small">
<!-- TODO: Add inputs for Date, Amount, Category, Description -->
<!-- Use lightning-input, lightning-combobox, and lightning-textarea -->
</div>


<div class="slds-p-around_small slds-align_absolute-center">
🧡 *Good luck, have fun, and remember: clean, readable code > clever code.*
