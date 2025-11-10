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
<targets>
<target>lightning__RecordPage</target>
<target>lightning__AppPage</target>
<target>lightning__HomePage</target>
</targets>
</LightningComponentBundle>
```


✅ **Checklist:**
- [ ] Data table displays expenses
- [ ] Clicking “View” opens the record


---


## 🧱 Step 5 — Lightning Page Customization


1. Go to **Lightning App Builder → Expense__c Record Page**
2. Add both components: `expenseCreator` and `expenseList`
3. Configure dynamic visibility if desired (e.g., only standard users can see the creator form)
4. Activate the page for your app


✅ **Checklist:**
- [ ] Page layout looks clean
- [ ] Both LWCs functional


---


## 🧾 Step 6 — Visualforce Page (Optional)


File: `ExpenseReportVF.page`
```html
<apex:page standardController="Expense__c" recordSetVar="expenses">
<h1>Expense Report</h1>
<apex:pageBlock title="All Expenses">
<apex:pageBlockTable value="{!expenses}" var="e">
<!-- TODO: Add columns for Name, Amount, Category, Status -->
</apex:pageBlockTable>
</apex:pageBlock>
<apex:commandButton value="Back to Lightning" action="/lightning/o/Expense__c/list"/>
</apex:page>
```


✅ **Checklist:**
- [ ] Page deployed
- [ ] Accessible via `/apex/ExpenseReportVF`


---


## 🧭 Step 7 — Custom Button or Quick Action


- Create a **Quick Action** labeled **Approve Expense**
- Action type: Flow or Apex
- Logic: Sets `Status__c = 'Approved'`
- Add to Expense__c page layout


✅ **Checklist:**
- [ ] Button visible and functional


---


## 🚀 Stretch Challenges


1. Add filtering by Category in the Expense List.
2. Add a Validation Rule so Amount__c must be greater than 0.
3. Add a Flow to auto-approve expenses under $100.
4. Add a Permission Set for managers.


---


## 🧩 Reflection Questions


- What’s the difference between using @AuraEnabled(cacheable=true) and a regular Apex method?
- Why is NavigationMixin useful in Lightning Web Components?
- How could this app be extended to support file attachments or receipts?


---


## ✅ Deliverables Summary


| Component | Purpose | Deliverable |
|------------|----------|-------------|
| Expense__c | Data Model | Object + Fields Created |
| ExpenseController | Logic | Apex Class with Methods |
| expenseCreator | UI | Form to Create Expense |
| expenseList | UI | Datatable to Display Expenses |
| ExpenseReportVF | Legacy UI | List of All Expenses |
| Approve Button | Automation | Status Change Action |


---


🧡 *Good luck, have fun, and remember: clean, readable code > clever code.*
