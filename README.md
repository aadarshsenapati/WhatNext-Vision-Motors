# WhatNext Vision Motors – Salesforce CRM Project

WhatsNext Vision Motors is revolutionizing its customer experience and operational efficiency with a cutting-edge Salesforce CRM implementation. This project streamlines the vehicle ordering process by auto-assigning orders to the nearest dealer based on customer location and preventing orders for out-of-stock vehicles.

Automated workflows update order statuses dynamically and send scheduled email reminders for test drives. Key technical implementations include Apex triggers for stock validation, batch jobs for stock updates, and scheduled Apex for automated order processing. This initiative enhances customer satisfaction, improves order accuracy, and boosts overall operational efficiency.

---

## Project Features

### Core Functionalities

- Automated dealer assignment based on customer location.
- Stock validation to prevent orders for unavailable vehicles.
- Dynamic order status updates.
- Scheduled email reminders for test drives.
- Real-time dashboards and reporting.

---

## Data Model

### Custom Objects

| Object Name                    | Purpose                        | Key Fields                                           |
|-------------------------------|--------------------------------|------------------------------------------------------|
| `Vehicle__c`                  | Stores vehicle details         | Vehicle Name, Model, Price, Stock, Status            |
| `Vehicle_Dealer__c`           | Authorized dealer information  | Dealer Name, Location, Phone, Email                  |
| `Vehicle_Customer__c`         | Customer information           | Name, Email, Phone, Address, Preferred Vehicle Type  |
| `Vehicle_Order__c`            | Tracks vehicle orders          | Vehicle, Customer, Status, Order Date                |
| `Vehicle_Test_Drive__c`       | Test drive bookings            | Vehicle, Customer, Test Drive Date, Status           |
| `Vehicle_Service_Request__c`  | Vehicle servicing requests     | Vehicle, Customer, Service Date, Issue, Status       |

---

## Lightning App Configuration

**App Name:** `WhatsNext Vision Motors`

### Tabs Included

- Vehicles  
- Dealers  
- Customers  
- Orders  
- Test Drives  
- Service Requests  
- Reports  
- Dashboards

These tabs are configured using custom object tabs and standard Salesforce features.

---

## Automation

### 1. Auto-Assign Nearest Dealer

- **Trigger:** When an order is created and status is set to `Pending`
- **Logic:**  
  - Retrieve customer address  
  - Match a dealer based on location  
  - Assign the dealer to the order

### 2. Test Drive Reminder

- **Trigger:** When a test drive is created or updated  
- **Scheduled Path:** 1 day before the scheduled test drive date (`Test_Drive_Date__c`)  
- **Action:** Send reminder email to the customer

---

## Apex Trigger Logic

### Trigger Handler: `VehicleOrderTriggerHandler`

- **Before Insert/Update:** Prevents order if the selected vehicle is out of stock.
- **After Insert/Update:** Deducts stock quantity when order status is confirmed.

```apex
trigger VehicleOrderTrigger on Vehicle_Order__c (before insert, before update, after insert, after update) {
    VehicleOrderTriggerHandler.handleTrigger(Trigger.new, Trigger.oldMap, Trigger.isBefore, Trigger.isAfter, Trigger.isInsert, Trigger.isUpdate);
}
```

## Batch Apex & Scheduling

### Batch Class: `VehicleOrderBatch`

- Scans all `Pending` orders
- If stock is available:
  - Updates order status to `Confirmed`
  - Decreases stock quantity accordingly

### Scheduler Class: `VehicleOrderBatchScheduler`

```apex
String cronExp = '0 0 12 * * ?';
System.schedule('Daily Vehicle Order Processing', cronExp, new VehicleOrderBatchScheduler());
```
## Dashboards and Reports

Custom reports and dashboards provide visibility into:

- Vehicle stock availability  
- Order status distribution  
- Scheduled test drives  
- Dealer performance  

These analytics support informed business decisions and proactive customer service.

---

## Deliverables

- Salesforce Lightning App  
- Six custom objects with defined relationships  
- Record-triggered flows  
- Apex triggers and handler classes  
- Batch and scheduled Apex classes for background automation  
- Custom reports and dashboards  

---

## Technologies Used

- Salesforce Lightning Platform  
- Apex (Triggers, Batch, and Scheduler)  
- Record-Triggered Flows  
- Scheduled Paths and Email Alerts  
- Standard and Custom Reports & Dashboards  

---

## Contact

For any queries, issues, or contributions, feel free to open a GitHub issue or reach out via LinkedIn or email.
