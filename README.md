# GarageFlow: A Vehicle Repair Management System

**Student:** Bizimana Credo  
**Student ID:** 20252SEN031  
**Course:** INSY 8311 – Database Development with PL/SQL (Final Exam capstone), Adventist University of Central Africa (AUCA)  
**Instructor:** Eric Maniraguha  

---

## 1. Problem statement

Vehicle ownership in Rwanda keeps growing. The National Institute of Statistics of Rwanda (NISR) reports 330,166 registered vehicles as of December 2023, and Rwanda Revenue Authority (RRA) figures reported in January 2026 put the total at 394,157 for 2024/25. More vehicles mean more repair work for garages.

Small and medium garages often record repair jobs on paper job cards or in spreadsheets. This makes it hard to trace the repair history of a vehicle, the parts and labour used on a job, and what a customer has paid. Parts run out without warning, and managers cannot easily see which jobs are late or unpaid.

GarageFlow is an Oracle database with PL/SQL that manages the whole repair process in a garage, from the arrival of a vehicle to repair, parts use and payment. Business rules are enforced inside the database, not left to staff memory.

## 2. Objectives

**Main objective:** Design and implement an Oracle PL/SQL database that manages vehicle repair jobs, parts and payments in a garage, using real repair data.

**Specific objectives**
1. Model the repair process and normalize the selected real-world dataset into well-structured tables in Third Normal Form (3NF).
2. Store customers, vehicles, job cards, mechanics, services, parts and payments with primary keys, foreign keys and integrity constraints.
3. Implement PL/SQL procedures, functions, cursors, a package and exception handling to open, update and close repair jobs and to compute costs.
4. Enforce garage business rules with triggers and keep an audit log of important changes.
5. Provide an Oracle APEX interface connected to the database so that staff can use the system beyond SQL Developer.

## 3. Data source

| Item | Detail |
|---|---|
| Dataset | Chinese Vehicle Fleet Fault Association Dataset (Zheng, 2026) |
| Link | https://data.mendeley.com/datasets/r98g7g6k5t/1 (DOI: 10.17632/r98g7g6k5t.1) |
| Content | Real-world vehicle fleet maintenance records from repair facilities in China, collected 2020–2023. Files: `vehicle_fleet.csv` (20 fields) and `fault_events.csv` (33 fields), plus annotation files |
| Licence | CC BY 4.0 (allows educational use with attribution) |
| Size | *Record counts to be confirmed after download (must be a few hundred records or more)* |
| Personal data | Vehicle and fault data only; any identifying fields will be removed or anonymized before loading |

The dataset has vehicles and fault events but no customers, parts stock or payments. These supporting reference data will be added in Phase II so that the database can represent a full garage workflow.

## 4. Innovation

**Option:** User interface  
**Tool:** Oracle APEX  
A service clerk registers a vehicle, opens a job card, adds parts and records a payment through an APEX application. The application calls the PL/SQL procedures, while the business rules stay in PL/SQL procedures and triggers. It will be demonstrated live on the Oracle database in Phase VII and documented in an `innovation/` folder.

## 5. Planned tables (draft, final design in Phase II)

`CUSTOMER`, `VEHICLE`, `MECHANIC`, `SERVICE_TYPE`, `JOB_CARD`, `JOB_SERVICE`, `PART`, `JOB_PART`, `PAYMENT`, `AUDIT_LOG`

- One customer owns many vehicles; one vehicle has many job cards.
- One mechanic is assigned to many job cards.
- A job card includes many services (through `JOB_SERVICE`) and many parts (through `JOB_PART`).
- A job card receives many payments.
- `AUDIT_LOG` records changes made to important tables.

## 6. Planned PL/SQL features

| Feature | Where it is used |
|---|---|
| Procedure `open_job_card` | Creates a job card when a vehicle arrives at the garage |
| Procedure `add_part_to_job` | Adds a part to a job and reduces stock |
| Procedure `record_payment` | Records a customer payment against a job card |
| Procedure `close_job_card` | Closes a job after work is complete and payment is settled |
| Function `calc_job_total` | Returns labour plus parts cost for a job |
| Function `next_service_due` | Returns the next service date for a vehicle |
| Cursors | List jobs open for too many days; list parts below reorder level |
| Package `garageflow_pkg` | Groups the procedures and functions above |
| Triggers | Block use of an out-of-stock part; block closing a job with an unpaid balance; stop changes to a closed job; write changes to `AUDIT_LOG` |
| Exceptions | `insufficient_stock`, `job_not_found`, `unpaid_balance` for clear error messages |


## 7. References (APA)

National Institute of Statistics of Rwanda. (n.d.). *ICT, water, sanitation, energy, tourism and transport* [Statistical publications]. https://www.statistics.gov.rw/statistical-publications/ict-water-sanitation-energy-tourism-and-transport

Oracle. (n.d.). PL/SQL triggers. In *Oracle Database PL/SQL language reference* (19c). https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/plsql-triggers.html

RTA Fleet. (n.d.). *Repair history by single vehicle* [Report documentation]. https://rtafleet.atlassian.net/wiki/x/LQAdsg

The New Times. (2026, January 21). *Vehicles registered in Rwanda pass 390,000*. https://www.newtimes.co.rw/article/32759/news/rwanda/vehicles-registered-in-rwanda-pass-390000
