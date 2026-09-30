Author: AAVA
Created on:
Description: Conceptual Data Model for the TMS Shipment reporting suite (Oracle TMS to MATM migration), covering five shipment reports.
Version: 1
Updated on:

# Conceptual Data Model – TMS Shipment Reporting (Oracle TMS to MATM Migration)

Source: `Input/Shipment_Report_Requirements.txt`. Only information in that file is used. Where an entity or grouping is inferred from the listed attributes, it is marked "(inferred)".

---

## 1. Domain Overview

**Primary business domain:** Transportation Management / Logistics – Shipment Operations.

**Supporting sub-domains**

| Sub-Domain | Description | Reports Served |
|---|---|---|
| Shipment Management | Lifecycle, status, type, and identifiers of shipments | All five reports |
| Carrier & Assignment Management | Primary, secondary, broker, designated and feasible carriers; tendering and pickup performance | Shipment Summary, Carrier Performance & Assignment, Distance & Route |
| Network / Facility & Stop Management | Origin and destination facilities derived from stop sequence (first stop = origin, last stop = destination) | Shipment Summary, Carrier Performance, Distance & Route |
| Route & Distance Analytics | Total, direct and out-of-route distance, stops, equipment | Carrier Performance, Distance & Route |
| Billing & Financial Reference | Bill-to party, billing method, bill of lading, business partner, purchase order, reconciliation | Bill-to & Financial Reference, Shipment Summary |
| Data Governance & Audit | Creation source, creator role, creation date | Shipment Creation & Source Audit |

**Reports in scope**
1. Shipment Summary Report
2. Carrier Performance & Assignment Report
3. Shipment Distance & Route Analysis Report
4. Bill-to & Financial Reference Report
5. Shipment Creation & Source Audit Report

**Context:** The requirements describe a migration of the Shipment table from Oracle TMS to MATM. Ten open questions (see Section 6.3) must be resolved before development and before moving to logical data modelling.

---

## 2. Entities

| # | Entity | Description | Basis |
|---|---|---|---|
| 1 | Shipment | Central entity. One movement of goods, with identifiers, status, type, leg type, flags and dates. | Stated |
| 2 | Carrier Assignment | The carriers linked to a shipment: primary (assigned), secondary, broker, designated (DC-to-Store master lane / static route) and feasible carriers, plus mode of transport. | Inferred grouping of stated carrier attributes |
| 3 | Carrier | A transportation provider that can be assigned to shipments. | Inferred |
| 4 | Stop | A stop on a shipment. Sequence determines origin (first) and destination (last). | Stated in data constraints |
| 5 | Facility | Physical location (origin or destination) with name, address, city, state, postal code, country. | Stated |
| 6 | Route & Distance | Distance measures and stop count for a shipment (total, direct, out-of-route, unit of measure). | Inferred grouping |
| 7 | Equipment | Equipment type and trailer used for the shipment. | Inferred grouping |
| 8 | Billing Reference | Bill-to postal code and state/province, bill of lading number, billing method, purchase order reference, reconciliation flag and date. | Inferred grouping |
| 9 | Business Partner (Vendor) | Partner/vendor associated with a shipment (held as an extended attribute in the target system). | Stated |
| 10 | Company | Company identifier that owns or is associated with the shipment. | Inferred from "company identifier" |
| 11 | Shipment Creation Audit | How, when and by whom a shipment was created (source, creation date/time, creator role). | Inferred grouping |
| 12 | User Role | Reference of users and their roles, used to resolve the creator role. | Stated (user role reference table) |

---

## 3. Attributes

Business names only; identifiers used as keys and data types are intentionally omitted. Business reference numbers that users see in the reports are included, because the requirements list them as report attributes.

### 3.1 Shipment
| Attribute | Description |
|---|---|
| Shipment Reference Number | Business reference of the shipment; must be unique per row at base grain. (Relationship to "TC Shipment" reference is an open question.) |
| TC Shipment Reference Number | Second shipment reference; mapping to the above to be validated with real data. |
| Parent Shipment Reference | Reference to a parent shipment. |
| Reference Shipment | Reference to a related shipment. |
| Shipment Status | Current status; must be a valid predefined domain value. |
| Shipment Type | Classification of the shipment. |
| Leg Type | Type of leg the shipment represents. |
| Shipment Creation Date | Date the shipment was created. |
| Cancelled Flag | Whether the shipment is cancelled (based on a specific planning status value in the source system). |
| Reconciled Flag | Whether the shipment has been reconciled. |
| Scheduled Pickup Date | Planned pickup date. |
| Shipment Age (Days) | Calculated: current date minus creation date. |

### 3.2 Carrier Assignment
| Attribute | Description |
|---|---|
| Assigned Primary Carrier | Primary carrier assigned to the shipment. |
| Secondary Carrier | Secondary carrier assigned. |
| Broker Carrier | Broker carrier assigned (priority status pending confirmation). |
| Designated Carrier | Master lane / static route carrier for DC-to-Store shipments; handled separately from standard assignment. |
| Feasible Carrier | Carrier(s) considered feasible for the shipment. |
| Mode of Transport | Mode used for the shipment. |
| Assigned Lane | Lane assigned (priority status pending confirmation). |

### 3.3 Carrier
| Attribute | Description |
|---|---|
| Carrier Name / Code | Name or code identifying the carrier (inferred from "carrier" fields; broker carrier code is mentioned in open questions). |

### 3.4 Stop
| Attribute | Description |
|---|---|
| Stop Sequence | Order of the stop on the shipment; first = origin, last = destination. |
| Number of Stops | Count of stops on the shipment. |

### 3.5 Facility (Origin / Destination)
| Attribute | Description |
|---|---|
| Facility Name | Name of the facility. |
| Address | Street address. |
| City | City. |
| State | State/province. |
| Postal Code | Postal code. |
| Country | Country. |
| Facility Role | Origin or Destination (derived from stop sequence). Booking facility fields are an open question. |

### 3.6 Route & Distance
| Attribute | Description |
|---|---|
| Total Distance | Total route distance; non-negative; > 0 for rate calculations. |
| Direct Distance | Direct distance between origin and destination; must be ≤ total distance. |
| Out-of-Route Distance | Distance travelled beyond the direct route; must not exceed total distance. |
| Distance Unit of Measure | Unit (miles or km); must be uniform across dataset. |
| Excess Distance | Calculated: total distance minus direct distance. |

### 3.7 Equipment
| Attribute | Description |
|---|---|
| Equipment Type | Type of equipment used. |
| Trailer Number | Trailer identifying number. |

### 3.8 Billing Reference
| Attribute | Description |
|---|---|
| Bill of Lading Number | Bill of lading number for the shipment. |
| Billing Method | Method of billing (numeric in legacy, string in new system; transformation rule needed). |
| Bill-to Postal Code | Postal code of bill-to party. |
| Bill-to State/Province | State or province of bill-to party. |
| Purchase Order Reference | Purchase order reference (target source not yet identified). |
| Reconciliation Date | Date reconciled, if available. |

### 3.9 Business Partner (Vendor)
| Attribute | Description |
|---|---|
| Business Partner / Vendor | Partner or vendor associated with the shipment (from extended attribute; extraction logic to be confirmed). |

### 3.10 Company
| Attribute | Description |
|---|---|
| Company | Company associated with the shipment. |

### 3.11 Shipment Creation Audit
| Attribute | Description |
|---|---|
| Creation Date and Time | When the shipment was created; must not be null. |
| Created Source | How the shipment was created (e.g., Manual, API, Integration); must not be null. |
| Created Source Type (Creator Role) | Role category of the creator; must not be null and must resolve via user role reference. |

### 3.12 User Role
| Attribute | Description |
|---|---|
| User | User who created or updated the shipment. |
| Role | Role of the user. |

---

## 4. KPIs

| # | KPI | Definition | Report(s) |
|---|---|---|---|
| 1 | Total Shipment Count | Count of shipments by status, type, and mode | Shipment Summary; Creation Audit (Total Shipments) |
| 2 | Cancelled Shipment % | Cancelled Shipments / Total Shipments × 100 | Shipment Summary |
| 3 | Reconciled Shipment % | Reconciled Shipments / Total Shipments × 100 | Shipment Summary; Bill-to & Financial |
| 4 | Shipments per Carrier | Shipments by assigned carrier | Shipment Summary |
| 5 | Shipments by Origin / Destination Facility | Count by facility | Shipment Summary |
| 6 | Shipment Age (Days) | Current date − creation date | Shipment Summary |
| 7 | Carrier Assignment Rate % | Assigned vs feasible carriers | Carrier Performance |
| 8 | Broker Carrier Usage % | Shipments with broker carrier / Total Shipments × 100 | Carrier Performance |
| 9 | On-time Pickup % | Scheduled pickup date versus actual | Carrier Performance |
| 10 | Out-of-Route Distance % | Out-of-route distance / Total distance × 100 | Carrier Performance; Distance & Route |
| 11 | Average Stops per Shipment | Total stops / Total shipments | Carrier Performance; Distance & Route |
| 12 | Shipments by Equipment Type | Count by equipment type | Carrier Performance |
| 13 | Average Distance per Shipment / Average Route Distance | Total distance / Count of shipments | Carrier Performance; Distance & Route |
| 14 | Average Direct Distance | Average of direct distance | Distance & Route |
| 15 | Route Efficiency Index | Direct distance / Total distance (closer to 1.0 = more efficient; valid range 0–1) | Distance & Route |
| 16 | Excess Distance | Total distance − Direct distance | Distance & Route |
| 17 | Zero-Distance Shipment Count | Reported separately; excluded from averages/rates | Distance & Route (from constraints) |
| 18 | Shipments by Billing Method | Count by billing method | Bill-to & Financial |
| 19 | Shipments by Business Partner (Vendor) | Count by partner | Bill-to & Financial |
| 20 | Unreconciled Shipment Count with Aging | Unreconciled shipments; aging days = current date − creation date | Bill-to & Financial |
| 21 | Shipments by Created Source | Count by source (Manual, API, Integration, etc.) | Creation Audit |
| 22 | Shipments by Created Source Type | Count by creator role category | Creation Audit |
| 23 | Creation Volume Trend / Creation Rate | Shipments by source per day or week | Creation Audit |
| 24 | Source Mix % | Count by source / Total count × 100 | Creation Audit |
| 25 | Shipments by Company | Count by company | Creation Audit |

---

## 5. Conceptual Data Model Diagram

Relationships and cardinalities are inferred from the report requirements; key fields are business-level fields.

| Source Entity | Relationship | Target Entity | Key Field | Cardinality |
|---|---|---|---|---|
| Shipment | has | Carrier Assignment | Shipment Reference Number | 1 : 1 (inferred) |
| Carrier Assignment | references (primary, secondary, broker, designated, feasible) | Carrier | Carrier Name / Code | Many : 1 per role (feasible may be Many : Many) |
| Shipment | consists of | Stop | Shipment Reference Number, Stop Sequence | 1 : Many |
| Stop | occurs at | Facility | Facility Name / Address | Many : 1 |
| Shipment | originates at (first stop) | Facility | Stop Sequence = first | Many : 1 |
| Shipment | terminates at (last stop) | Facility | Stop Sequence = last | Many : 1 |
| Shipment | measured by | Route & Distance | Shipment Reference Number | 1 : 1 (inferred) |
| Shipment | uses | Equipment | Equipment Type, Trailer Number | Many : 1 (inferred) |
| Shipment | billed via | Billing Reference | Bill of Lading Number | 1 : 1 (inferred) |
| Billing Reference | associated with | Business Partner (Vendor) | Business Partner / Vendor | Many : 1 |
| Shipment | belongs to | Company | Company | Many : 1 |
| Shipment | is created per | Shipment Creation Audit | Shipment Reference Number | 1 : 1 |
| Shipment Creation Audit | resolved by | User Role | User / Creator Role | Many : 1 (user must exist in reference) |
| Shipment | is child of | Shipment (Parent) | Parent Shipment Reference | Many : 1 (optional) |

**Text overview**

```
Company ──< Shipment >── Business Partner (via Billing Reference)
                │
   ┌────────────┼──────────────┬───────────────┬────────────────┐
   │            │              │               │                │
Carrier     Stop >── Facility  Route &      Equipment     Shipment Creation
Assignment  (first=origin,     Distance                   Audit >── User Role
   │         last=destination)
 Carrier                        Billing Reference
```

---

## 6. Common Data Elements Across Reports

### 6.1 Shared data elements

| Data Element | Entity | Shipment Summary | Carrier Performance | Distance & Route | Bill-to & Financial | Creation Audit |
|---|---|:-:|:-:|:-:|:-:|:-:|
| Shipment Reference Number | Shipment | ✔ | ✔ | ✔ | ✔ | ✔ |
| Shipment Status | Shipment | ✔ | ✔ | ✔ | ✔ | ✔ |
| Shipment Type | Shipment | ✔ | ✔ | ✔ | | ✔ |
| Parent Shipment Reference | Shipment | | | | ✔ | ✔ |
| Shipment Creation Date | Shipment | ✔ | | | (used for aging) | ✔ |
| Reconciled Flag | Shipment | ✔ | | | ✔ | |
| Assigned Primary Carrier | Carrier Assignment | ✔ | ✔ | ✔ | | |
| Secondary Carrier | Carrier Assignment | ✔ | ✔ | | | ✔ |
| Broker Carrier | Carrier Assignment | ✔ | ✔ | | | |
| Designated Carrier | Carrier Assignment | | ✔ | ✔ | | |
| Mode of Transport | Carrier Assignment | ✔ | ✔ | ✔ | | |
| Bill of Lading Number | Billing Reference | ✔ | ✔ | | ✔ | |
| Company | Company | ✔ | | | ✔ | ✔ |
| Trailer Number | Equipment | ✔ | ✔ | | | |
| Equipment Type | Equipment | | ✔ | ✔ | | |
| Origin / Destination Facility (city, state, country) | Facility | ✔ | ✔ (Origin/Destination slicer) | ✔ | | |
| Total Distance | Route & Distance | | ✔ | ✔ | | |
| Direct Distance | Route & Distance | | ✔ | ✔ | | |
| Out-of-Route Distance | Route & Distance | | ✔ | ✔ | | |
| Distance Unit of Measure | Route & Distance | | ✔ | ✔ | | |
| Number of Stops | Stop | | ✔ | ✔ | | |
| Created Source / Creator Role | Shipment Creation Audit | ✔ | | | | ✔ |

### 6.2 Shared KPIs / calculations
- Reconciled Shipment % (Shipment Summary, Bill-to & Financial)
- Out-of-Route Distance % (Carrier Performance, Distance & Route)
- Average Stops per Shipment (Carrier Performance, Distance & Route)
- Average Distance (Carrier Performance, Distance & Route)
- Total Shipment Count (Shipment Summary, Creation Audit)
- Shipment Age / Unreconciled Aging (Shipment Summary, Bill-to & Financial)

### 6.3 Open questions affecting the model (from the requirements)
1. Broker Carrier fields – priority reporting usage to be confirmed.
2. Assigned Lane – priority status to be confirmed.
3. Billing Method – datatype difference (legacy numeric vs new string); transformation rule required.
4. Booking Facility fields (origin/destination) – usage to be confirmed.
5. Cancelled Shipment Flag – legacy to next-generation planning status mapping (e.g., 0 = 00800, 1 = 0500) to be confirmed with real data.
6. Number of Docks – not found in target system; skipped for now (not modelled).
7. Shipment ID vs TC Shipment ID – to be disambiguated with real data.
8. Purchase Order – no source identified in target system.
9. Business Partner / Vendor ID – extended attribute extraction logic to be confirmed.
10. Baseline Cost – excluded for now (not modelled); final business decision pending.

### 6.4 Security notes (from the requirements)
Access varies by report and role (e.g., logistics managers and above for carrier/shipment data; regional operations leads for facility drill-down; finance for billing, rate and cost data; data governance/BI for full audit data). Detailed row/column-level rules belong to later design phases.

---

## 7. API Cost Calculation

apiCost: computed by the AAVA coordinator from token usage (see run report)
