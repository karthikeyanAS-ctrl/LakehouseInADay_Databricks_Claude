Author: AAVA
Created on:
Description: Databricks Bronze layer logical data model for the TMS Shipment application (SHIPMENT table), with PII classification, audit table design, conceptual relationships and API cost.
Version: 1
Updated on:

# Databricks Bronze Layer Logical Data Model – TMS Shipment

Source model: `Input/Shipment_Process_Table.txt` (one source entity: SHIPMENT).
Reference: `Output/Conceptual/Shipment_Report_Requirements_Conceptual_1.md`.

**Modelling notes (stated plainly)**
- The source model defines exactly one table, SHIPMENT. The Bronze layer therefore has one table, `Bz_Shipment`. No other entities or columns are added.
- The only key declared in the source is the primary key `SHIPMENT_ID`; it is excluded from `Bz_Shipment`. The source declares **no foreign key constraints**, so no column is excluded as a foreign key. Other `*_ID` / `*_CODE` columns (e.g. ASSIGNED_CARRIER_ID, CUSTOMER_ID, TC_SHIPMENT_ID) are kept as ordinary business attributes. This is an inference: they look like references to other entities, but the source does not declare them as keys.
- Bronze keeps the source columns as-is (no cleansing or business rules). Source columns flagged "(not used)", "(removed from reporting)" or "(source field not yet identified)" (e.g. BASELINE_COST, NUM_DOCKS) are still kept, because the source model lists them. The conceptual model marks Baseline Cost and Number of Docks as not modelled for reporting; that decision applies to later layers, not to raw ingestion.
- Data type mapping: VARCHAR(n) → STRING, DATETIME → TIMESTAMP, DECIMAL(p,s) → DECIMAL(p,s), INT → INT.
- All source columns except the primary key are kept. Nullability is not restated in Bronze (Bronze is a raw landing zone). In the source, CREATED_DTTM, CREATED_SOURCE, SHIPMENT_STATUS, SHIPMENT_TYPE, TC_COMPANY_ID and TC_SHIPMENT_ID are Not Null.

---

## 1. PII Classification

Classification is inferred from column names and business descriptions only. The source model has no PII tags. It should be confirmed by data governance.

| Column | PII Category | Reason |
|---|---|---|
| BILL_TO_NAME | Direct identifier (personal / party name) | Name of the bill-to party. It can be an individual's name. |
| BILL_TO_TITLE | Personal attribute | Title or salutation of the bill-to contact, which identifies a person. |
| BILL_TO_PHONE_NUMBER | Contact information | Phone number of the bill-to party. |
| BILL_TO_ADDRESS | Contact information / location | Street address of the bill-to party. |
| BILL_TO_CITY | Location (quasi-identifier) | City of the bill-to party. |
| BILL_TO_STATE_PROV | Location (quasi-identifier) | State or province of the bill-to party. |
| BILL_TO_POSTAL_CODE | Location (quasi-identifier) | Postal code of the bill-to party. It is granular enough to help identify an individual. |
| BILL_TO_COUNTRY_CODE | Location (low sensitivity) | Country of the bill-to party. |
| BILL_TO_CODE | Indirect identifier | Code identifying the bill-to party. |
| HAZMAT_CERT_CONTACT | Contact information | Contact information for hazmat certification. It can be a person's name, phone number or email. |
| TRANS_PLAN_OWNER | Personal name / user identifier | Owner or planner responsible for the transportation plan. It may hold a person's name or user ID. |
| CREATED_SOURCE | Possible user identifier | System or process that created the record. It may hold a user ID when created manually. |
| CREATED_SOURCE_TYPE | Role (low sensitivity) | Role type of the creating user or system. |
| LAST_UPDATED_SOURCE | Possible user identifier | System or process that last updated the record. It may hold a user ID. |
| LAST_UPDATED_SOURCE_TYPE | Role (low sensitivity) | Role type of the updating user or system. |
| CUSTOMER_ID, ASSIGNED_CUSTOMER_ID, CUSTOMER_CREDIT_LIMIT_ID | Indirect identifier | Identify the customer on the shipment. |
| BUSINESS_PARTNER_ID | Indirect identifier | Identifies a business partner or vendor. |
| O_ADDRESS, O_CITY, O_COUNTY, O_STATE_PROV, O_POSTAL_CODE, O_COUNTRY_CODE, O_STOP_LOCATION_NAME | Location (business, low sensitivity) | Origin facility address details. They are normally business locations. They become PII only if a stop is a residence. |
| D_ADDRESS, D_CITY, D_COUNTY, D_STATE_PROV, D_POSTAL_CODE, D_COUNTRY_CODE, D_STOP_LOCATION_NAME | Location (business, low sensitivity) | Destination facility address details. Same reasoning as the origin columns. Delivery to a residence could make them PII. |
| BK_RESOURCE_NAME_EXTERNAL | Possible personal name | External resource name on the booking. It may hold a person's or company name. |
| DESIGNATED_DRIVER_TYPE, DRIVER_TYPE_ID, FEASIBLE_DRIVER_TYPE | Not PII | These hold a driver type or category, not a driver identity. |

All other columns are treated as non-PII operational, cost, status, date, and reference data.

---

## 2. Bronze Layer Logical Model

### 2.1 Table: Bz_Shipment

Source table: SHIPMENT. Excluded: SHIPMENT_ID (primary key). The three metadata columns at the end are added by the Bronze layer and do not exist in the source.

#### Business columns (source columns, in source order)

| Column Name | Data Type | Business Description |
|---|---|---|
| ACCESSORIAL_COST | DECIMAL(10,2) | Total accessorial charges applied to the shipment |
| ACCESSORIAL_COST_TO_CARRIER | DECIMAL(10,2) | Accessorial cost passed to the carrier |
| ACTUAL_COST | DECIMAL(10,2) | Actual total cost of the shipment |
| ACTUAL_COST_CURRENCY_CODE | STRING | Currency code for the actual shipment cost |
| APPT_DOOR_SCHED_TYPE | STRING | Appointment door scheduling type |
| ASSIGNED_BROKER_CARRIER_CODE | STRING | Code identifying the broker carrier assigned to the shipment |
| ASSIGNED_BROKER_CARRIER_ID | STRING | Unique identifier for the broker carrier assigned to the shipment |
| ASSIGNED_CARRIER_CODE | STRING | Code identifying the primary carrier assigned to the shipment |
| ASSIGNED_CARRIER_ID | STRING | Unique identifier for the primary carrier assigned to the shipment |
| ASSIGNED_CM_SHIPMENT_ID | STRING | Identifier for the carrier management shipment assignment |
| ASSIGNED_CUSTOMER_ID | STRING | Identifier for the customer assigned to the shipment |
| ASSIGNED_EQUIPMENT_ID | STRING | Identifier for the equipment assigned to the shipment |
| ASSIGNED_LANE_DETAIL_ID | STRING | Identifier for the detail record of the assigned lane |
| ASSIGNED_LANE_ID | STRING | Identifier for the lane assigned to the shipment |
| ASSIGNED_MOT_ID | STRING | Identifier for the mode of transport assigned to the shipment |
| ASSIGNED_SCNDR_CARRIER_CODE | STRING | Code identifying the secondary carrier assigned to the shipment |
| ASSIGNED_SCNDR_CARRIER_ID | STRING | Unique identifier for the secondary carrier assigned to the shipment |
| ASSIGNED_SERVICE_LEVEL_ID | STRING | Identifier for the service level assigned to the shipment |
| ASSIGNED_SHIP_VIA | STRING | Ship-via method assigned to the shipment |
| AUTH_NBR | STRING | Authorization number associated with the shipment |
| AVAILABLE_DTTM | TIMESTAMP | Date and time when the shipment became available |
| BASELINE_COST | DECIMAL(10,2) | Baseline cost used for cost comparison (removed from reporting) |
| BASELINE_COST_CURRENCY_CODE | STRING | Currency code for the baseline shipment cost |
| BILL_OF_LADING_NUMBER | STRING | Bill of lading reference number for the shipment |
| BILL_TO_ADDRESS | STRING | Street address of the bill-to party |
| BILL_TO_CITY | STRING | City of the bill-to party |
| BILL_TO_CODE | STRING | Code identifying the bill-to party |
| BILL_TO_COUNTRY_CODE | STRING | Country code of the bill-to party |
| BILL_TO_NAME | STRING | Name of the bill-to party |
| BILL_TO_PHONE_NUMBER | STRING | Phone number of the bill-to party |
| BILL_TO_POSTAL_CODE | STRING | Postal code of the bill-to party |
| BILL_TO_STATE_PROV | STRING | State or province of the bill-to party |
| BILL_TO_TITLE | STRING | Title or salutation of the bill-to contact |
| BILLING_METHOD | STRING | Method used to bill the shipment |
| BK_ARRIVAL_DTTM | TIMESTAMP | Booking arrival date and time |
| BK_ARRIVAL_TZ | STRING | Timezone for the booking arrival date and time |
| BK_CUTOFF_DTTM | TIMESTAMP | Booking cutoff date and time |
| BK_CUTOFF_TZ | STRING | Timezone for the booking cutoff date and time |
| BK_D_FACILITY_ALIAS_ID | STRING | Alias identifier for the booking destination facility |
| BK_D_FACILITY_ID | STRING | Identifier for the booking destination facility |
| BK_DEPARTURE_DTTM | TIMESTAMP | Booking departure date and time |
| BK_DEPARTURE_TZ | STRING | Timezone for the booking departure date and time |
| BK_FORWARDER_AIRWAY_BILL | STRING | Forwarder airway bill number from booking |
| BK_MASTER_AIRWAY_BILL | STRING | Master airway bill number from booking |
| BK_O_FACILITY_ALIAS_ID | STRING | Alias identifier for the booking origin facility |
| BK_O_FACILITY_ID | STRING | Identifier for the booking origin facility |
| BK_PICKUP_DTTM | TIMESTAMP | Booking pickup date and time |
| BK_PICKUP_TZ | STRING | Timezone for the booking pickup date and time |
| BK_RESOURCE_NAME_EXTERNAL | STRING | External resource name associated with the booking |
| BK_RESOURCE_REF_EXTERNAL | STRING | External resource reference associated with the booking |
| BOOKING_ID | STRING | Unique identifier for the freight booking |
| BOOKING_REF_CARRIER | STRING | Carrier-side reference number for the booking |
| BOOKING_REF_SHIPPER | STRING | Shipper-side reference number for the booking |
| BROKER_CARRIER_ID | STRING | Identifier for the broker carrier associated with the shipment |
| BROKER_REF | STRING | Broker reference number for the shipment |
| BUDG_CM_DISCOUNT | DECIMAL(10,2) | Budget carrier management discount applied to the shipment |
| BUDG_CURRENCY_CODE | STRING | Currency code for the budget cost fields |
| BUDG_NORMALIZED_TOTAL_COST | DECIMAL(10,2) | Normalized total budgeted cost (not used) |
| BUDG_TOTAL_COST | DECIMAL(10,2) | Total budgeted cost for the shipment (not used) |
| BUSINESS_PARTNER_ID | STRING | Identifier for the business partner or vendor linked to the shipment |
| BUSINESS_PROCESS | STRING | Business process category associated with the shipment |
| CARRIER_CHARGE | DECIMAL(10,2) | Charge amount billed by the carrier |
| CFMF_STATUS | STRING | Status of the CFMF (carrier freight management flow) process |
| CM_DISCOUNT | DECIMAL(10,2) | Carrier management discount applied to the shipment |
| CMID | STRING | Carrier management identifier for the shipment |
| COD_AMOUNT | DECIMAL(10,2) | Cash on delivery amount for the shipment |
| COD_CURRENCY_CODE | STRING | Currency code for the cash on delivery amount |
| COMMODITY_CLASS | STRING | Freight commodity class for rating purposes |
| COMMODITY_CODE_ID | STRING | Identifier for the commodity code assigned to the shipment |
| CONFIG_CYCLE_SEQ | INT | Configuration cycle sequence number for optimization processing |
| CONS_ADDR_CODE | STRING | Consolidation address code associated with the shipment |
| CONS_LOCN_ID | STRING | Consolidation location identifier |
| CONS_RUN_ID | STRING | Consolidation run identifier |
| CONTRACT_NUMBER | STRING | Contract number governing the shipment pricing |
| COST_BREAKUP | STRING | Detailed breakdown of shipment cost components |
| CREATED_DTTM | TIMESTAMP | Date and time when the shipment record was created (Not Null in source) |
| CREATED_SOURCE | STRING | System or process that created the shipment record (Not Null in source) |
| CREATED_SOURCE_TYPE | STRING | Role type of the user or system that created the shipment |
| CREATION_TYPE | STRING | Type of creation process used to generate the shipment |
| CURRENCY_CODE | STRING | Default currency code used for this shipment |
| CURRENCY_DTTM | TIMESTAMP | Date and time of the currency rate used for this shipment |
| CUST_FRGT_CHARGE | DECIMAL(10,2) | Customer freight charge applied to the shipment |
| CUSTOMER_CREDIT_LIMIT_ID | STRING | Credit limit identifier for the customer on this shipment |
| CUSTOMER_ID | STRING | Identifier for the customer associated with the shipment |
| CYCLE_DEADLINE_DTTM | TIMESTAMP | Optimization cycle deadline date and time |
| CYCLE_EXECUTION_DTTM | TIMESTAMP | Date and time the optimization cycle was executed |
| CYCLE_RESP_DEADLINE_TZ | STRING | Timezone for the cycle response deadline |
| D_ADDRESS | STRING | Street address of the destination facility |
| D_CITY | STRING | City of the destination facility |
| D_COUNTRY_CODE | STRING | Country code of the destination facility |
| D_COUNTY | STRING | County of the destination facility |
| D_FACILITY_ID | STRING | Unique identifier for the destination facility (last stop) |
| D_FACILITY_NUMBER | STRING | Facility number of the destination (last stop) |
| D_POSTAL_CODE | STRING | Postal code of the destination facility |
| D_STATE_PROV | STRING | State or province of the destination facility |
| D_STOP_LOCATION_NAME | STRING | Name of the destination stop location |
| D_TANDEM_FACILITY | STRING | Tandem facility identifier at the destination |
| D_TANDEM_FACILITY_ALIAS | STRING | Alias for the tandem facility at the destination |
| DAYS_TO_DELIVER | INT | Number of days planned or actual for delivery |
| DECLARED_VALUE | DECIMAL(10,2) | Declared monetary value of the shipment contents |
| DELAY_TYPE | STRING | Type of delay associated with the shipment |
| DELIVERY_END_DTTM | TIMESTAMP | Planned delivery window end date and time |
| DELIVERY_REQ | STRING | Delivery requirements or special instructions |
| DELIVERY_START_DTTM | TIMESTAMP | Planned delivery window start date and time |
| DELIVERY_TZ | STRING | Timezone for the delivery window |
| DESIGNATED_DRIVER_TYPE | STRING | Type of driver designated for the shipment |
| DESIGNATED_TRACTOR_CODE | STRING | Code for the tractor designated for the shipment |
| DIRECT_DISTANCE | DECIMAL(10,2) | Straight-line distance between origin and destination |
| DISTANCE | DECIMAL(10,2) | Total route distance for the shipment |
| DISTANCE_UOM | STRING | Unit of measure for the shipment distance. Domain values: MI, KM |
| DOOR | STRING | Door number assigned to the shipment at the facility |
| DRIVER_TYPE_ID | STRING | Identifier for the type of driver required |
| DROPOFF_PICKUP | STRING | Indicates whether the shipment is a drop-off or pickup. Domain values: DROPOFF, PICKUP |
| DSG_CARRIER_CODE | STRING | Designated carrier code; for DC-to-Store used as Master Lane or Static Route reference |
| DSG_CARRIER_ID | STRING | Unique identifier for the designated carrier |
| DSG_EQUIPMENT_ID | STRING | Identifier for the designated equipment |
| DSG_MOT_ID | STRING | Identifier for the designated mode of transport |
| DSG_SCNDR_CARRIER_CODE | STRING | Code for the designated secondary carrier |
| DSG_SCNDR_CARRIER_ID | STRING | Identifier for the designated secondary carrier |
| DSG_SERVICE_LEVEL_ID | STRING | Identifier for the designated service level |
| DSG_VOYAGE_FLIGHT | STRING | Voyage or flight number for the designated booking |
| DT_PARAM_SET_ID | STRING | Date-time parameter set identifier used in scheduling |
| DV_CURRENCY_CODE | STRING | Currency code for the declared value |
| EARNED_INCOME | DECIMAL(10,2) | Income earned on the shipment |
| EARNED_INCOME_CURRENCY_CODE | STRING | Currency code for the earned income amount |
| EQUIP_UTIL_PER | DECIMAL(5,2) | Equipment utilization percentage for the shipment. Domain values: 0.0 to 100.0 |
| EQUIPMENT_TYPE | STRING | Type of equipment used or required for the shipment |
| ESTIMATED_COST | DECIMAL(10,2) | Estimated cost of the shipment prior to execution |
| ESTIMATED_DISPATCH_DTTM | TIMESTAMP | Estimated date and time for dispatch |
| ESTIMATED_SAVINGS | DECIMAL(10,2) | Estimated cost savings achieved on the shipment |
| EVENT_IND_TYPEID | STRING | Event indicator type identifier for the shipment |
| EXT_SYS_SHIPMENT_ID | STRING | Shipment identifier from an external system |
| EXTRACTION_DTTM | TIMESTAMP | Date and time when the record was extracted from the source system |
| FACILITY_SCHEDULE_ID | STRING | Identifier for the facility schedule linked to the shipment |
| FEASIBLE_CARRIER_CODE | STRING | Code of a carrier identified as feasible during optimization |
| FEASIBLE_CARRIER_ID | STRING | Identifier for the feasible carrier identified during optimization |
| FEASIBLE_DRIVER_TYPE | STRING | Driver type identified as feasible for the shipment |
| FEASIBLE_EQUIPMENT_ID | STRING | Primary equipment identifier identified as feasible |
| FEASIBLE_EQUIPMENT2_ID | STRING | Secondary equipment identifier identified as feasible |
| FEASIBLE_MOT_ID | STRING | Mode of transport identified as feasible for the shipment |
| FEASIBLE_SERVICE_LEVEL_ID | STRING | Service level identified as feasible for the shipment |
| FEASIBLE_VOYAGE_FLIGHT | STRING | Voyage or flight identified as feasible for the shipment |
| FINANCIAL_WT | DECIMAL(10,3) | Financial weight used for cost calculation purposes |
| FIRST_UPDATE_SENT_TO_PKMS | STRING | Indicator whether the first update was sent to the warehouse system. Domain values: Y, N |
| FRT_REV_ACCESSORIAL_CHARGE | DECIMAL(10,2) | Freight revenue accessorial charge amount |
| FRT_REV_CM_DISCOUNT | DECIMAL(10,2) | Freight revenue carrier management discount |
| FRT_REV_LINEHAUL_CHARGE | DECIMAL(10,2) | Freight revenue linehaul charge amount |
| FRT_REV_RATING_LANE_DETAIL_ID | STRING | Rating lane detail identifier for freight revenue |
| FRT_REV_RATING_LANE_ID | STRING | Rating lane identifier for freight revenue |
| FRT_REV_SPOT_CHARGE | DECIMAL(10,2) | Spot charge amount for freight revenue |
| FRT_REV_SPOT_CHARGE_CURR_CODE | STRING | Currency code for the freight revenue spot charge |
| FRT_REV_STOP_CHARGE | DECIMAL(10,2) | Stop charge amount for freight revenue |
| GRS_MAX_SHIPMENT_STATUS | STRING | Maximum shipment status within a global routing session |
| HAS_ALERTS | STRING | Indicates whether the shipment has active alerts. Domain values: Y, N |
| HAS_EM_NOTIFY_FLAG | STRING | Flag indicating whether event management notifications are active. Domain values: Y, N |
| HAS_IMPORT_ERROR | STRING | Flag indicating whether an import error exists on the shipment. Domain values: Y, N |
| HAS_NOTES | STRING | Indicates whether notes have been added to the shipment. Domain values: Y, N |
| HAS_SOFT_CHECK_ERROR | STRING | Flag indicating a soft validation check error exists. Domain values: Y, N |
| HAS_TRACKING_MSG | STRING | Indicates whether tracking messages exist for the shipment. Domain values: Y, N |
| HAULING_CARRIER | STRING | Carrier physically hauling the shipment |
| HAZMAT_CERT_CONTACT | STRING | Contact information for hazmat certification |
| HAZMAT_CERT_DECLARATION | STRING | Hazmat certification declaration statement |
| HIBERNATE_VERSION | INT | Hibernate ORM version number for the record |
| HUB_ID | STRING | Identifier for the hub facility associated with the shipment |
| INBOUND_REGION_ID | STRING | Region identifier for the inbound leg of the shipment |
| INCOTERM_ID | STRING | Incoterm governing the terms of trade for the shipment |
| INSURANCE_STATUS | STRING | Status of insurance coverage for the shipment |
| IS_ASSOCIATED_TO_OUTBOUND | STRING | Flag indicating whether the shipment is linked to an outbound shipment. Domain values: Y, N |
| IS_AUTO_DELIVERED | STRING | Flag indicating the shipment was automatically marked as delivered. Domain values: Y, N |
| IS_BOOKING_REQUIRED | STRING | Flag indicating whether a booking is required for the shipment. Domain values: Y, N |
| IS_CM_OPTION_GEN_ACTIVE | STRING | Flag indicating carrier management option generation is active. Domain values: Y, N |
| IS_COOLER_AT_NOSE | STRING | Flag indicating cooler unit is required at the nose of the trailer. Domain values: Y, N |
| IS_FILO | STRING | Flag indicating first-in last-out loading sequence. Domain values: Y, N |
| IS_GRS_OPT_CYCLE_RUNNING | STRING | Flag indicating a global routing session optimization cycle is running. Domain values: Y, N |
| IS_HAZMAT | STRING | Flag indicating the shipment contains hazardous materials. Domain values: Y, N |
| IS_MANUAL_ASSIGN | STRING | Flag indicating the carrier was manually assigned. Domain values: Y, N |
| IS_MISROUTED | STRING | Flag indicating the shipment was identified as misrouted. Domain values: Y, N |
| IS_PERISHABLE | STRING | Flag indicating the shipment contains perishable goods. Domain values: Y, N |
| IS_SHIPMENT_CANCELLED | STRING | Flag indicating the shipment has been cancelled. Domain values: Y, N |
| IS_SHIPMENT_RECONCILED | STRING | Flag indicating the shipment has been financially reconciled. Domain values: Y, N |
| IS_TIME_FEAS_ENABLED | STRING | Flag indicating time feasibility checks are enabled. Domain values: Y, N |
| IS_WAVE_MAN_CHANGED | STRING | Flag indicating the wave management assignment was manually changed. Domain values: Y, N |
| LANE_NAME | STRING | Name of the lane assigned to the shipment |
| LAST_CM_OPTION_GEN_DTTM | TIMESTAMP | Date and time of the last carrier management option generation |
| LAST_RS_NOTIFICATION_DTTM | TIMESTAMP | Date and time of the last routing session notification |
| LAST_RUN_GRS_DTTM | TIMESTAMP | Date and time of the last global routing session run |
| LAST_SELECTOR_RUN_DTTM | TIMESTAMP | Date and time of the last carrier selector run |
| LAST_UPDATED_DTTM | TIMESTAMP | Date and time when the shipment record was last updated |
| LAST_UPDATED_SOURCE | STRING | System or process that last updated the shipment record |
| LAST_UPDATED_SOURCE_TYPE | STRING | Role type of the user or system that last updated the shipment |
| LEFT_WT | DECIMAL(10,3) | Weight on the left side of the load for balance calculation |
| LH_PAYEE_CARRIER_CODE | STRING | Code for the carrier that is the payee for linehaul charges |
| LH_PAYEE_CARRIER_ID | STRING | Identifier for the carrier that is the payee for linehaul charges |
| LINEHAUL_COST | DECIMAL(10,2) | Total linehaul freight cost for the shipment |
| LOADING_SEQ_ORD | INT | Loading sequence order for the shipment on the vehicle |
| LOC_REFERENCE | STRING | Location reference identifier associated with the shipment |
| LPN_ASSIGNMENT_STOPPED | STRING | Flag indicating LPN assignment processing has been stopped. Domain values: Y, N |
| MANIFEST_ID | STRING | Identifier for the manifest this shipment belongs to |
| MARGIN | DECIMAL(10,2) | Margin amount calculated for the shipment |
| MAX_NBR_OF_CTNS | INT | Maximum number of cartons allowed on the shipment |
| MERCHANDIZING_DEPARTMENT_ID | STRING | Merchandising department identifier linked to the shipment |
| MIN_RATE | DECIMAL(10,2) | Minimum rate applicable to the shipment |
| MONETARY_VALUE | DECIMAL(10,2) | Total monetary value of the shipment contents |
| MOVE_TYPE | STRING | Type of movement for the shipment (e.g., inbound, outbound, transfer) |
| MV_CURRENCY_CODE | STRING | Currency code for the monetary value of the shipment |
| NORM_SPOT_CHARGE_AND_PAYEE_ACC | DECIMAL(10,2) | Normalized total of spot charge and payee accessorial amounts |
| NORMALIZED_BASELINE_COST | DECIMAL(10,2) | Normalized baseline cost for cross-currency comparison |
| NORMALIZED_MARGIN | DECIMAL(10,2) | Normalized margin for cross-currency comparison (not used) |
| NORMALIZED_TOTAL_COST | DECIMAL(10,2) | Normalized total shipment cost for cross-currency comparison (not used) |
| NORMALIZED_TOTAL_REVENUE | DECIMAL(10,2) | Normalized total revenue for cross-currency comparison |
| NUM_CHARGE_LAYOVERS | INT | Number of charge-incurring layovers on the shipment route |
| NUM_DOCKS | INT | Number of dock doors used for the shipment (source field not yet identified) |
| NUM_STOPS | INT | Total number of stops on the shipment route |
| O_ADDRESS | STRING | Street address of the origin facility |
| O_CITY | STRING | City of the origin facility |
| O_COUNTRY_CODE | STRING | Country code of the origin facility |
| O_COUNTY | STRING | County of the origin facility |
| O_FACILITY_ID | STRING | Unique identifier for the origin facility (first stop) |
| O_FACILITY_NUMBER | STRING | Facility number of the origin (first stop) |
| O_POSTAL_CODE | STRING | Postal code of the origin facility |
| O_STATE_PROV | STRING | State or province of the origin facility |
| O_STOP_LOCATION_NAME | STRING | Name of the origin stop location |
| O_TANDEM_FACILITY | STRING | Tandem facility identifier at the origin |
| O_TANDEM_FACILITY_ALIAS | STRING | Alias for the tandem facility at the origin |
| OCEAN_ROUTING_STAGE | STRING | Stage in the ocean routing process for this shipment |
| ON_TIME_INDICATOR | STRING | Indicates whether the shipment was delivered on time. Domain values: ON_TIME, LATE, EARLY |
| ORDER_QTY | DECIMAL(10,3) | Order quantity associated with the shipment |
| ORIG_BUDG_TOTAL_COST | DECIMAL(10,2) | Original budgeted total cost before any revisions |
| OUT_OF_ROUTE_DISTANCE | DECIMAL(10,2) | Distance travelled beyond the direct route |
| OUTBOUND_REGION_ID | STRING | Region identifier for the outbound leg of the shipment |
| PACKAGING | STRING | Packaging type or description for the shipment |
| PAPERWORK_START_DTTM | TIMESTAMP | Date and time when shipment paperwork processing began |
| PAYEE_CARRIER_ID | STRING | Identifier for the carrier that will receive payment |
| PICK_START_DATE | TIMESTAMP | Date when picking of the shipment began |
| PICKUP_END_DTTM | TIMESTAMP | Planned pickup window end date and time |
| PICKUP_START_DATE | TIMESTAMP | Planned pickup window start date and time |
| PICKUP_TZ | STRING | Timezone for the pickup window |
| PLANNED_VOLUME | DECIMAL(10,3) | Planned volume of the shipment |
| PLANNED_WEIGHT | DECIMAL(10,3) | Planned weight of the shipment |
| PLN_ACCESSORL_COST_TO_CARRIER | DECIMAL(10,2) | Planned accessorial cost to be passed to the carrier |
| PLN_CARRIER_CHARGE | DECIMAL(10,2) | Planned carrier charge for the shipment |
| PLN_CURRENCY_CODE | STRING | Currency code for the planned cost fields |
| PLN_LINEHAUL_COST | DECIMAL(10,2) | Planned linehaul cost for the shipment |
| PLN_MAX_TEMPERATURE | DECIMAL(5,2) | Maximum planned temperature for temperature-controlled shipments |
| PLN_MIN_TEMPERATURE | DECIMAL(5,2) | Minimum planned temperature for temperature-controlled shipments |
| PLN_NORMALIZED_TOTAL_COST | DECIMAL(10,2) | Normalized planned total cost for cross-currency comparison |
| PLN_RATING_LANE_DETAIL_ID | STRING | Rating lane detail identifier used in planned cost calculation |
| PLN_RATING_LANE_ID | STRING | Rating lane identifier used in planned cost calculation |
| PLN_STOP_OFF_COST | DECIMAL(10,2) | Planned stop-off charge for the shipment |
| PLN_TOTAL_ACCESSORIAL_COST | DECIMAL(10,2) | Planned total accessorial cost for the shipment |
| PLN_TOTAL_COST | DECIMAL(10,2) | Planned total cost for the shipment |
| PP_SHIPMENT_ID | STRING | Parent or predecessor shipment identifier |
| PRINT_CONS_BOL | STRING | Flag indicating whether a consolidated bill of lading should be printed. Domain values: Y, N |
| PRIORITY_TYPE | STRING | Priority classification of the shipment |
| PRO_NUMBER | STRING | Progressive (PRO) number assigned by the carrier |
| PROD_SCHED_REF_NUMBER | STRING | Production schedule reference number linked to the shipment |
| PRODUCT_CLASS_ID | STRING | Product class identifier for the goods in the shipment |
| PROTECTION_LEVEL_ID | STRING | Protection level required for the shipment contents |
| PURCHASE_ORDER | STRING | Purchase order number associated with the shipment |
| QTY_UOM_ID | STRING | Unit of measure identifier for quantity fields |
| RADIAL_DISTANCE | DECIMAL(10,2) | Radial (straight-line) distance from a reference point |
| RADIAL_DISTANCE_UOM | STRING | Unit of measure for the radial distance. Domain values: MI, KM |
| RATE | DECIMAL(10,4) | Rate applied to the shipment for cost calculation |
| RATE_TYPE | STRING | Type of rate applied to the shipment (e.g., flat, per mile) |
| RATE_UOM | STRING | Unit of measure for the applied rate |
| RATING_LANE_DETAIL_ID | STRING | Detail-level identifier for the rating lane used |
| RATING_LANE_ID | STRING | Identifier for the rating lane used for cost calculation |
| RATING_QUALIFIER | STRING | Qualifier used to determine the applicable rate |
| REC_ACCESSORIAL_COST | DECIMAL(10,2) | Recommended accessorial cost for the shipment |
| REC_BROKER_CARRIER_CODE | STRING | Recommended broker carrier code |
| REC_BROKER_CARRIER_ID | STRING | Recommended broker carrier identifier |
| REC_BUDG_ACCESSORIAL_COST | DECIMAL(10,2) | Recommended budget accessorial cost |
| REC_BUDG_CM_DISCOUNT | DECIMAL(10,2) | Recommended budget carrier management discount |
| REC_BUDG_CURRENCY_CODE | STRING | Currency code for the recommended budget fields |
| REC_BUDG_LINEHAUL_COST | DECIMAL(10,2) | Recommended budget linehaul cost (not used) |
| REC_BUDG_NORMALIZED_TOTAL_COST | DECIMAL(10,2) | Recommended normalized total budget cost |
| REC_BUDG_RATING_LANE_DETAIL_ID | STRING | Rating lane detail identifier for the recommended budget |
| REC_BUDG_RATING_LANE_ID | STRING | Rating lane identifier for the recommended budget |
| REC_BUDG_STOP_COST | DECIMAL(10,2) | Recommended budget stop cost |
| REC_BUDG_TOTAL_COST | DECIMAL(10,2) | Recommended total budget cost for the shipment |
| REC_CARRIER_CODE | STRING | Code of the recommended carrier |
| REC_CARRIER_ID | STRING | Identifier for the recommended carrier |
| REC_CM_DISCOUNT | DECIMAL(10,2) | Recommended carrier management discount |
| REC_CM_SHIPMENT_ID | STRING | Recommended carrier management shipment identifier |
| REC_CMID | STRING | Recommended carrier management identifier |
| REC_COST_BREAKUP | STRING | Detailed breakdown of the recommended shipment cost |
| REC_CURRENCY_CODE | STRING | Currency code for the recommended cost fields |
| REC_EQUIPMENT_ID | STRING | Recommended equipment identifier |
| REC_LANE_DETAIL_ID | STRING | Detail-level identifier for the recommended lane |
| REC_LANE_ID | STRING | Identifier for the recommended lane |
| REC_LINEHAUL_COST | DECIMAL(10,2) | Recommended linehaul cost for the shipment |
| REC_MARGIN | DECIMAL(10,2) | Margin on the recommended cost option |
| REC_MOT_ID | STRING | Recommended mode of transport identifier |
| REC_NORMALIZED_MARGIN | DECIMAL(10,2) | Normalized margin on the recommended option |
| REC_NORMALIZED_TOTAL_COST | DECIMAL(10,2) | Normalized total cost for the recommended option |
| REC_RATING_LANE_DETAIL_ID | STRING | Rating lane detail identifier for the recommended option |
| REC_RATING_LANE_ID | STRING | Rating lane identifier for the recommended option |
| REC_SERVICE_LEVEL_ID | STRING | Recommended service level identifier |
| REC_SPOT_CHARGE | DECIMAL(10,2) | Recommended spot charge amount |
| REC_SPOT_CHARGE_CURRENCY_CODE | STRING | Currency code for the recommended spot charge |
| REC_STOP_COST | DECIMAL(10,2) | Recommended stop charge cost |
| REC_TOTAL_COST | DECIMAL(10,2) | Recommended total cost for the shipment |
| RECEIVED_DTTM | TIMESTAMP | Date and time when the shipment was received at the destination |
| REF_SHIPMENT_NBR | STRING | Reference shipment number linked to this shipment |
| REGION_ID | STRING | Region identifier associated with the shipment |
| REPORTED_COST | DECIMAL(10,2) | Cost reported for the shipment after execution |
| RETAIN_CONSOLIDATOR_TIMES | STRING | Flag indicating consolidator times should be retained during re-planning. Domain values: Y, N |
| REVENUE_RATING_LEVEL | STRING | Rating level used to determine revenue for the shipment |
| RIGHT_WT | DECIMAL(10,3) | Weight on the right side of the load for balance calculation |
| RS_AREA_ID | STRING | Routing session area identifier |
| RS_CONFIG_CYCLE_ID | STRING | Routing session configuration cycle identifier |
| RS_CONFIG_ID | STRING | Routing session configuration identifier |
| RS_CYCLE_REMAINING | INT | Number of optimization cycles remaining in the routing session |
| RS_TYPE | STRING | Type of routing session applied to the shipment |
| RTE_SWC_NBR | STRING | Route switch number for the shipment |
| RTE_TO | STRING | Route-to destination code |
| RTE_TYPE | STRING | Primary route type for the shipment |
| RTE_TYPE_1 | STRING | Secondary route type classification |
| RTE_TYPE_2 | STRING | Tertiary route type classification |
| SCHEDULED_PICKUP_DTTM | TIMESTAMP | Scheduled date and time for pickup |
| SCNDR_CARRIER_ID | STRING | Identifier for the secondary carrier on the shipment |
| SEAL_NUMBER | STRING | Seal number applied to the shipment trailer or container |
| SED_GENERATED_FLAG | STRING | Flag indicating a Shipper's Export Declaration was generated. Domain values: Y, N |
| SENT_TO_CREATE_PKMS | STRING | Flag indicating the create instruction was sent to the warehouse system. Domain values: Y, N |
| SENT_TO_CREATE_PKMS_DTTM | TIMESTAMP | Date and time the create instruction was sent to the warehouse system |
| SENT_TO_PKMS | STRING | Flag indicating an update was sent to the warehouse system. Domain values: Y, N |
| SENT_TO_PKMS_DTTM | TIMESTAMP | Date and time the update was sent to the warehouse system |
| SERV_AREA_CODE | STRING | Service area code associated with the shipment |
| SHIP_GROUP_ID | STRING | Identifier for the shipment group this shipment belongs to |
| SHIPMENT_CLOSED_INDICATOR | STRING | Flag indicating the shipment has been closed. Domain values: Y, N |
| SHIPMENT_END_DTTM | TIMESTAMP | Date and time when the shipment execution ended |
| SHIPMENT_LEG_TYPE | STRING | Type of shipment leg (e.g., direct, relay, multi-modal) |
| SHIPMENT_RECON_DTTM | TIMESTAMP | Date and time when the shipment was financially reconciled |
| SHIPMENT_REF_ID | STRING | Reference identifier linking related shipments |
| SHIPMENT_START_DTTM | TIMESTAMP | Date and time when shipment execution began |
| SHIPMENT_STATUS | STRING | Current transit status of the shipment (Not Null in source). Domain values: PLANNED, IN_TRANSIT, DELIVERED, CANCELLED |
| SHIPMENT_TYPE | STRING | Product class type of the shipment (Not Null in source) |
| SHIPMENT_WIN_ADJ_FLAG | STRING | Flag indicating a window adjustment was made to the shipment. Domain values: Y, N |
| SIZE1_UOM_ID | STRING | Unit of measure for the first size dimension |
| SIZE1_VALUE | DECIMAL(10,3) | Value of the first size dimension |
| SIZE2_UOM_ID | STRING | Unit of measure for the second size dimension |
| SIZE2_VALUE | DECIMAL(10,3) | Value of the second size dimension |
| SPOT_CHARGE | DECIMAL(10,2) | Spot charge amount applied to the shipment |
| SPOT_CHARGE_AND_PAYEE_ACC | DECIMAL(10,2) | Combined spot charge and payee accessorial amount |
| SPOT_CHARGE_AND_PAYEE_ACC_CC | STRING | Currency code for the combined spot charge and payee accessorial |
| SPOT_CHARGE_CURRENCY_CODE | STRING | Currency code for the spot charge amount |
| STAGING_LOCN_ID | STRING | Identifier for the staging location for the shipment |
| STATIC_ROUTE_ID | STRING | Identifier for the static route assigned to the shipment |
| STATUS_CHANGE_DATE | TIMESTAMP | Date when the shipment status last changed (not used) |
| STOP_COST | DECIMAL(10,2) | Stop charge cost for the shipment |
| TANDEM_PATH_ID | STRING | Identifier for the tandem routing path |
| TARIFF | STRING | Tariff code or schedule applied to the shipment |
| TC_COMPANY_ID | STRING | Company identifier within the TMS system (Not Null in source) |
| TC_SHIPMENT_ID | STRING | TMS-assigned shipment identifier (Not Null in source) |
| TEMPERATURE_UOM | STRING | Unit of measure for temperature fields on the shipment. Domain values: C, F |
| TENDER_DTTM | TIMESTAMP | Date and time when the shipment was tendered to the carrier |
| TENDER_RESP_DEADLINE_DATE | TIMESTAMP | Deadline date for the carrier to respond to the tender |
| TENDER_RESP_DEADLINE_TZ | STRING | Timezone for the tender response deadline |
| TOTAL_COST | DECIMAL(10,2) | Total shipment cost including all charges |
| TOTAL_COST_EXCL_TAX | DECIMAL(10,2) | Total shipment cost excluding tax |
| TOTAL_REVENUE | DECIMAL(10,2) | Total revenue generated by the shipment |
| TOTAL_REVENUE_CURRENCY_CODE | STRING | Currency code for the total revenue amount |
| TOTAL_TAX_AMOUNT | DECIMAL(10,2) | Total tax amount applied to the shipment |
| TOTAL_TIME | DECIMAL(10,2) | Total elapsed time for the shipment from pickup to delivery |
| TRACKING_MSG_PROBLEM | STRING | Flag or description of a problem with tracking messages |
| TRACTOR_NUMBER | STRING | Tractor unit number assigned to the shipment |
| TRAILER_NUMBER | STRING | Trailer number assigned to the shipment |
| TRANS_PLAN_OWNER | STRING | Owner or planner responsible for the transportation plan |
| TRANS_RESP_CODE | STRING | Transport responsibility code indicating who manages the freight |
| TRLR_GEN_CODE | STRING | Trailer generation code |
| TRLR_SIZE | STRING | Size category of the trailer assigned to the shipment |
| TRLR_TYPE | STRING | Type of trailer assigned to the shipment |
| UN_NUMBER_ID | STRING | UN hazmat number identifier for dangerous goods |
| UPDATE_SENT | STRING | Flag indicating an update notification was sent for the shipment. Domain values: Y, N |
| USE_BROKER_AS_CARRIER | STRING | Flag indicating the broker should be treated as the carrier. Domain values: Y, N |
| VEHICLE_CHECK_START_DTTM | TIMESTAMP | Date and time when the vehicle check process started |
| VOLUME_UOM_ID_BASE | STRING | Base unit of measure identifier for volume fields |
| WAVE_ID | STRING | Wave planning identifier associated with the shipment |
| WAYPOINT_HANDLING_COST | DECIMAL(10,2) | Handling cost at waypoint stops along the route |
| WAYPOINT_TOTAL_COST | DECIMAL(10,2) | Total cost including all waypoint charges |
| WEIGHT_UOM_ID_BASE | STRING | Base unit of measure identifier for weight fields |
| WMS_STATUS_CODE | STRING | Status code from the warehouse management system for this shipment |

#### Bronze metadata columns (added by the Bronze layer)

| Column Name | Data Type | Business Description |
|---|---|---|
| load_timestamp | TIMESTAMP | Date and time when the record was first loaded into the Bronze layer |
| update_timestamp | TIMESTAMP | Date and time when the Bronze record was last updated. This is separate from the source column LAST_UPDATED_DTTM. |
| source_system | STRING | Name of the source system the record came from (TMS Shipment application) |

---

## 3. Audit Table Design

Table: `Bz_Audit_Log`. It records each Bronze load for the tables in this model.

| Column Name | Data Type | Business Description |
|---|---|---|
| record_id | STRING | Unique identifier of the audit record |
| source_table | STRING | Name of the source or Bronze table processed (e.g., SHIPMENT / Bz_Shipment) |
| load_timestamp | TIMESTAMP | Date and time when the load started |
| processed_by | STRING | Name of the user, job, or pipeline that ran the load |
| processing_time | DECIMAL(10,2) | Duration of the load (in seconds; the unit is an inference) |
| status | STRING | Outcome of the load (e.g., SUCCESS, FAILED). The values are illustrative; the source defines none. |

---

## 4. Conceptual Data Model Diagram (Tabular Form)

The source model has one table, so the Bronze layer has no physical relationships between Bronze tables. The relationships below come from the conceptual model. They are shown at Bronze level only through the reference columns of `Bz_Shipment`, which are not declared as foreign keys in the source. Cardinalities are inferred.

| Source Table | Relationship | Related Concept (conceptual model) | Linking Columns in Bz_Shipment | Cardinality |
|---|---|---|---|---|
| Bz_Shipment | assigned to | Carrier Assignment / Carrier | ASSIGNED_CARRIER_ID, ASSIGNED_SCNDR_CARRIER_ID, ASSIGNED_BROKER_CARRIER_ID, DSG_CARRIER_ID, FEASIBLE_CARRIER_ID | Many : 1 per role |
| Bz_Shipment | originates at | Facility (first stop) | O_FACILITY_ID | Many : 1 |
| Bz_Shipment | terminates at | Facility (last stop) | D_FACILITY_ID | Many : 1 |
| Bz_Shipment | measured by | Route & Distance | DISTANCE, DIRECT_DISTANCE, OUT_OF_ROUTE_DISTANCE, NUM_STOPS | 1 : 1 (columns held in the same table) |
| Bz_Shipment | uses | Equipment | EQUIPMENT_TYPE, TRAILER_NUMBER | Many : 1 |
| Bz_Shipment | billed via | Billing Reference | BILL_OF_LADING_NUMBER, BILLING_METHOD, BILL_TO_* | 1 : 1 (columns held in the same table) |
| Bz_Shipment | linked to | Business Partner (Vendor) | BUSINESS_PARTNER_ID | Many : 1 |
| Bz_Shipment | belongs to | Company | TC_COMPANY_ID | Many : 1 |
| Bz_Shipment | created per | Shipment Creation Audit | CREATED_DTTM, CREATED_SOURCE, CREATED_SOURCE_TYPE | 1 : 1 (columns held in the same table) |
| Bz_Shipment | is child of | Bz_Shipment (parent) | PP_SHIPMENT_ID | Many : 1 (optional) |
| Bz_Audit_Log | tracks loads of | Bz_Shipment | source_table | Many : 1 |

---

## 5. API Cost Calculation

apiCost: computed by the AAVA coordinator from token usage (see run report)
