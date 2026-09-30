Author: AAVA
Created on:
Description: Model Data Constraints for the TMS Shipment reporting suite (Oracle TMS to MATM migration), covering five shipment reports.
Version: 1
Updated on:

# Model Data Constraints – TMS Shipment Reporting (Oracle TMS to MATM Migration)

Source: Input/Shipment_Report_Requirements.txt. Entity names follow Output/Conceptual/Shipment_Report_Requirements_Conceptual_1.md (Shipment, Carrier Assignment, Carrier, Stop, Facility, Route & Distance, Equipment, Billing Reference, Business Partner (Vendor), Company, Shipment Creation Audit, User Role). Only information present in the requirements is used. Items that are inferences are marked "(inferred)".

---

## 1 Data Expectations

1.1 Scope and grain
- 1.1.1 The data comes from the TMS Shipment table, migrated from Oracle TMS to MATM.
- 1.1.2 The base grain is one row per shipment (Shipment entity).
- 1.1.3 Five reports are supported: (1) Shipment Summary, (2) Carrier Performance & Assignment, (3) Shipment Distance & Route Analysis, (4) Bill-to & Financial Reference, (5) Shipment Creation & Source Audit.

1.2 Expected data by entity
- 1.2.1 Shipment: shipment identifiers and reference numbers (Shipment Reference Number, TC Shipment Reference Number, Parent Shipment Reference, Reference Shipment), shipment status, shipment type, leg type, shipment creation date, cancelled flag, reconciled flag, scheduled pickup date.
- 1.2.2 Carrier Assignment: assigned primary carrier, secondary carrier, broker carrier, designated carrier (DC-to-Store master lane / static route reference), feasible carrier, mode of transport, assigned lane.
- 1.2.3 Stop: stop sequence and number of stops.
- 1.2.4 Facility (origin and destination): facility name, address, city, state, postal code, country.
- 1.2.5 Route & Distance: total distance, direct distance, out-of-route distance, distance unit of measure.
- 1.2.6 Equipment: equipment type, trailer number.
- 1.2.7 Billing Reference: bill of lading number, billing method, bill-to postal code, bill-to state/province, purchase order reference, reconciliation date (if available).
- 1.2.8 Business Partner (Vendor): business partner / vendor identifier.
- 1.2.9 Company: company identifier.
- 1.2.10 Shipment Creation Audit: creation date and time, creation source, creator role.
- 1.2.11 User Role: user-to-role reference used to derive the creator role.

1.3 Expected data by report
- 1.3.1 Shipment Summary: identifiers and reference numbers; status, type, leg type; origin and destination facility details; assigned primary carrier and mode of transport; secondary and broker carriers; bill of lading number, company identifier, trailer number; creation date, creation source, creator role; cancelled and reconciled flags.
- 1.3.2 Carrier Performance & Assignment: shipment identifier, status, type; assigned primary carrier, mode; secondary, broker, designated and feasible carriers; scheduled pickup date; bill of lading number; total distance, direct distance, distance unit of measure, out-of-route distance; equipment type, trailer number, number of stops.
- 1.3.3 Shipment Distance & Route Analysis: shipment identifier, status, type; origin and destination city, state, country; total route distance, direct distance, out-of-route distance, distance unit of measure; number of stops, mode of transport, equipment type; assigned carrier and designated carrier.
- 1.3.4 Bill-to & Financial Reference: shipment identifier and status; bill-to postal code and state/province; bill of lading number and billing method; business partner / vendor identifier; parent and reference shipment identifiers; purchase order reference; company identifier; reconciled flag and reconciliation date (if available).
- 1.3.5 Shipment Creation & Source Audit: shipment identifier, status, type; creation date and time, creation source, creator role; company identifier and parent shipment identifier; secondary carrier.

1.4 Expected KPIs and calculations
- 1.4.1 Total Shipment Count (by status, type, and mode).
- 1.4.2 Cancelled Shipment % = Cancelled Shipments / Total Shipments × 100.
- 1.4.3 Reconciled Shipment % = Reconciled Shipments / Total Shipments × 100.
- 1.4.4 Shipments per Carrier; Shipments by Origin and Destination Facility.
- 1.4.5 Shipment Age (Days) = Current date minus shipment creation date.
- 1.4.6 Carrier Assignment Rate % (assigned vs feasible).
- 1.4.7 Broker Carrier Usage % = Shipments with a broker carrier / Total Shipments × 100.
- 1.4.8 On-time Pickup % based on scheduled pickup date versus actual.
- 1.4.9 Out-of-Route Distance % = Out-of-route distance / Total distance × 100.
- 1.4.10 Average Stops per Shipment = Total stops / Total shipments.
- 1.4.11 Shipments by Equipment Type.
- 1.4.12 Average Distance per Shipment = Total distance / Count of shipments; Average Route Distance (miles or km); Average Direct Distance.
- 1.4.13 Route Efficiency Index = Direct distance / Total distance (closer to 1.0 = more efficient).
- 1.4.14 Excess Distance = Total distance minus Direct distance.
- 1.4.15 Shipments by Billing Method; Shipments by Business Partner (Vendor).
- 1.4.16 Unreconciled Shipment Count with aging; Unreconciled Aging Days = Current date minus shipment creation date (where not reconciled).
- 1.4.17 Shipments by Created Source; Shipments by Created Source Type; Creation Volume Trend (daily and weekly); Creation Rate = Count of shipments by source per day or week; Source Mix % = Count by source / Total count × 100; Shipments by Company.

1.5 Expected interactivity that depends on the data
- 1.5.1 Shipment Summary: drill down Network → Region → Facility → Shipment; drill through from shipment row to stop and carrier detail; slicers Shipment Status, Shipment Type, Mode of Transport, Carrier, Date Range, Origin/Destination Facility.
- 1.5.2 Carrier Performance: drill down Carrier → Lane → Shipment; drill through from KPI to shipment records; slicers Carrier, Mode of Transport, Equipment Type, Date Range, Origin/Destination.
- 1.5.3 Distance & Route: drill down Region → Lane → Shipment; drill through from lane summary to shipment distance details; slicers Origin/Destination State, Carrier, Mode of Transport, Date Range.
- 1.5.4 Bill-to & Financial: drill down Business Partner → Shipment; drill through from billing summary to shipment detail; slicers Billing Method, Business Partner, Shipment Status, Date Range, Company.
- 1.5.5 Creation Audit: drill down Source Type → Source → Shipment; drill through from source summary to shipment creation records; slicers Created Source, Source Type, Company, Date Range.

---

## 2 Constraints

2.1 Shipment identity and status
- 2.1.1 The shipment identifier must be unique per row at the base grain.
- 2.1.2 Shipment status must be a valid predefined domain value.
- 2.1.3 Cancelled shipments are identified by a specific planning status value in the source system. The mapping of legacy planning status codes to next-generation codes (e.g., 0 = 00800, 1 = 0500) must be confirmed with a real data example.
- 2.1.4 The two shipment identifier fields (Shipment ID and TC Shipment ID) must be disambiguated using a real data example, and the mapping between the primary key and the shipment ID field must be validated.

2.2 Origin and destination (Stop / Facility)
- 2.2.1 Origin and destination facility data must be derived using stop sequence logic: first stop = origin, last stop = destination.
- 2.2.2 Facility address joins must use the correct stop sequence logic.

2.3 Carrier Assignment
- 2.3.1 The designated carrier field for DC-to-Store shipments represents the Master Lane or Static Route and must be handled separately from standard carrier assignment logic.
- 2.3.2 Broker carrier fields (code and ID) and Assigned Lane: priority reporting status is unconfirmed (see 2.8) and must be confirmed with the business.

2.4 Route & Distance
- 2.4.1 Distance values must be non-negative.
- 2.4.2 Total distance must be greater than zero for rate calculations; zero-distance shipments are excluded from rate calculations and averages but counted and reported separately.
- 2.4.3 Distance unit of measure must be consistent across all rows; mixed units must be flagged, and converted where mixed units are present.
- 2.4.4 Direct distance must be less than or equal to total distance; exceptions are flagged.
- 2.4.5 Out-of-route distance must not exceed total route distance; anomalies are flagged.
- 2.4.6 Route Efficiency Index must be between 0 and 1.

2.5 Billing Reference and Business Partner
- 2.5.1 The business partner identifier is sourced from an extended attribute field; the extraction logic must be confirmed.
- 2.5.2 Billing method has a datatype difference between the legacy system (numeric) and the new system (string); a transformation rule is required and must be confirmed before development.
- 2.5.3 The purchase order source in the target system has not been identified; the source field and table must be confirmed before the field is included in the Bill-to & Financial Reference Report.

2.6 Shipment Creation Audit and User Role
- 2.6.1 Creation source and creator role must be non-null for audit completeness.
- 2.6.2 Creator role is derived via a join to the user role reference table; the user must exist in that table, and every shipment must have a resolvable creator role.
- 2.6.3 Creation date must not be null and must fall within the expected operational date range.

2.7 Count integrity
- 2.7.1 Cancelled shipment count must not exceed total shipment count.

2.8 Fields excluded or unresolved (from Open Questions)
- 2.8.1 Number of Docks: referenced in reports but not found in the target system; skipped for now unless a source can be identified.
- 2.8.2 Baseline Cost: excluded for now as it does not appear to be used in any active reports; final decision to be confirmed by the business.
- 2.8.3 Booking Facility fields (origin and destination): flagged as not used in priority reporting in the source system notes but as used by the SCDE team; confirmation needed.
- 2.8.4 Broker carrier code/ID: source system notes say "Not used in priority reporting" but the reporting team flagged them as used; confirmation needed.
- 2.8.5 Assigned Lane: not in the source system priority reporting list but flagged as used by the SCDE team; priority status to be confirmed.

2.9 Validations required
- 2.9.1 Cancelled shipment count does not exceed total shipment count.
- 2.9.2 Confirm the mapping between the two shipment identifier fields using a real data example.
- 2.9.3 Ensure facility address joins use the correct stop sequence logic.
- 2.9.4 Confirm priority status of broker carrier and assigned lane fields.
- 2.9.5 Out-of-route distance does not exceed total route distance; flag anomalies.
- 2.9.6 Distance unit of measure is uniform across the dataset.
- 2.9.7 Route Efficiency Index is between 0 and 1.
- 2.9.8 Zero-distance shipments are excluded from averages but reported separately.
- 2.9.9 Direct distance does not exceed total route distance.
- 2.9.10 Confirm the billing method datatype transformation rule before development.
- 2.9.11 Confirm the business partner extended attribute extraction logic.
- 2.9.12 Confirm the purchase order source field and table in the target system.
- 2.9.13 Every shipment has a resolvable creator role via the user role reference join.
- 2.9.14 Creation date is not null and falls within the expected operational date range.

2.10 Security and access constraints
- 2.10.1 Shipment Summary: carrier and shipment data visible to logistics managers and above; facility-level drill-down restricted to regional operations leads; corporate supply chain leadership can view the full network.
- 2.10.2 Carrier Performance: carrier performance data visible to logistics, procurement, and operations leadership; rate and cost data restricted to finance and senior leadership.
- 2.10.3 Distance & Route: accessible to logistics planning, network design, and carrier management teams; facility-level data restricted by regional hierarchy.
- 2.10.4 Bill-to & Financial: finance and accounts receivable teams can view full billing data; operations teams see shipment-level reference fields only; vendor and partner financial data restricted to finance leadership.
- 2.10.5 Creation Audit: data governance and BI teams can view full audit data; operations teams see aggregate views only; employee-level creation data restricted by role.

---

## 3 Business Rules

3.1 KPI and calculation rules
- 3.1.1 Cancelled Shipment % = Count of cancelled shipments / Total shipments × 100.
- 3.1.2 Reconciled % = Count of reconciled shipments / Total shipments × 100.
- 3.1.3 Shipment Age (Days) = Current date minus shipment creation date.
- 3.1.4 Unreconciled Aging Days = Current date minus shipment creation date, applied only where the shipment is not reconciled.
- 3.1.5 Broker Usage % = Count of shipments with a broker carrier assigned / Total shipments × 100.
- 3.1.6 Out-of-Route % = Out-of-route distance / Total distance × 100.
- 3.1.7 Average Distance per Shipment = Total distance / Count of shipments.
- 3.1.8 Average Stops per Shipment = Total stops / Total shipments.
- 3.1.9 Route Efficiency Index = Direct distance / Total distance (closer to 1.0 = more efficient).
- 3.1.10 Excess Distance = Total distance minus Direct distance.
- 3.1.11 Creation Rate = Count of shipments by source per day or week.
- 3.1.12 Source Mix % = Count by source / Total count × 100.
- 3.1.13 Carrier Assignment Rate % compares assigned versus feasible carriers; On-time Pickup % compares scheduled pickup date versus actual. The requirements give no formula beyond these descriptions.

3.2 Derivation rules
- 3.2.1 Origin facility = facility of the first stop; destination facility = facility of the last stop.
- 3.2.2 Creator role is obtained by joining the shipment creator to the user role reference table.
- 3.2.3 Cancelled status is derived from a specific planning status value in the source system (legacy to next-generation mapping to be confirmed).
- 3.2.4 Business partner / vendor is extracted from an extended attribute in the target system.

3.3 Handling rules
- 3.3.1 Designated carrier (DC-to-Store Master Lane / Static Route) is processed separately from standard carrier assignment.
- 3.3.2 Zero-distance shipments are excluded from rate calculations and averages, and counted separately.
- 3.3.3 Mixed distance units are flagged and converted so the unit is uniform.
- 3.3.4 Billing method is transformed between legacy (numeric) and new (string) representation according to a rule to be defined.
- 3.3.5 Number of Docks and Baseline Cost are excluded for now.

3.4 Interactivity and presentation rules
- 3.4.1 Drill-down paths, drill-through targets and slicers are as listed in 1.5.
- 3.4.2 Indicative layouts: each report has a top KPI area, a middle chart area and a bottom detail table with filter panel. Shipment Summary: KPI cards (Total Shipments, Active, Cancelled, Reconciled), status bar chart and mode of transport pie chart. Carrier Performance: KPI strip (Carrier count, On-time Pickup %, Average Distance, Broker Usage %) and carrier assignment heatmap by lane. Distance & Route: KPI cards (Average Distance, Average Out-of-Route %, Route Efficiency Index) and scatter plot of direct versus actual distance by lane. Bill-to & Financial: KPI cards (Reconciled %, Unreconciled Count, Distinct Billing Methods), billing method breakdown and partner distribution charts. Creation Audit: KPI cards (Total Shipments, Distinct Sources, Top Source Type), creation volume trend line and source breakdown bar chart.

3.5 Governance rules
- 3.5.1 All ten open questions must be resolved before report development begins.
- 3.5.2 Walkthrough with Business, SCDE / Reporting, Data Engineering, and Architecture teams; sign-off from all stakeholder groups is required before the logical data modelling phase.
- 3.5.3 Walkthrough feedback is incorporated into an updated version of the requirements document.

---

## 4 API Cost Calculation

apiCost: computed by the AAVA coordinator from token usage (see run report)
