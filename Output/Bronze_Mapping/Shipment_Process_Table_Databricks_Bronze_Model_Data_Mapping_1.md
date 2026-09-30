Author: AAVA
Created on:
Description: Databricks Bronze layer data mapping (one-to-one, no transformations) from the TMS Shipment source table SHIPMENT to workspace.bronze.bz_shipment, plus the bz_audit_log table.
Version: 1
Updated on:

# Databricks Bronze Layer Data Mapping – TMS Shipment

Inputs:
- Physical model: `Output/Bronze_Physical/Shipment_Process_Table_Databricks_Bronze_Model_Physical_1.md`
- Source data model: `Input/Shipment_Process_Table.txt`

Target: catalog `workspace`, schema `bronze`. Source: table SHIPMENT of the TMS Shipment application.

## 1. Mapping Rules

- Bronze is a raw landing layer. Every source column is copied one-to-one, with the same name and no transformation, cleansing, type conversion beyond the physical type mapping (VARCHAR → STRING, DATETIME → TIMESTAMP), or filtering.
- The source primary key SHIPMENT_ID is not carried into Bronze (as decided in the logical and physical models), so it has no row in the mapping.
- bz_shipment has 385 columns: 382 source columns + 3 metadata columns. Each appears exactly once below.
- bz_audit_log has 7 columns, all system generated / parameter driven. It has no source table column.

## 2. Data Mapping – bz_shipment (source columns)

| Target Layer | Target Table | Target Field | Source Layer | Source Table | Source Field | Transformation Rule |
|---|---|---|---|---|---|---|
| Bronze | bz_shipment | ACCESSORIAL_COST | Source | SHIPMENT | ACCESSORIAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | ACCESSORIAL_COST_TO_CARRIER | Source | SHIPMENT | ACCESSORIAL_COST_TO_CARRIER | 1-1 Mapping |
| Bronze | bz_shipment | ACTUAL_COST | Source | SHIPMENT | ACTUAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | ACTUAL_COST_CURRENCY_CODE | Source | SHIPMENT | ACTUAL_COST_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | APPT_DOOR_SCHED_TYPE | Source | SHIPMENT | APPT_DOOR_SCHED_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_BROKER_CARRIER_CODE | Source | SHIPMENT | ASSIGNED_BROKER_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_BROKER_CARRIER_ID | Source | SHIPMENT | ASSIGNED_BROKER_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_CARRIER_CODE | Source | SHIPMENT | ASSIGNED_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_CARRIER_ID | Source | SHIPMENT | ASSIGNED_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_CM_SHIPMENT_ID | Source | SHIPMENT | ASSIGNED_CM_SHIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_CUSTOMER_ID | Source | SHIPMENT | ASSIGNED_CUSTOMER_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_EQUIPMENT_ID | Source | SHIPMENT | ASSIGNED_EQUIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_LANE_DETAIL_ID | Source | SHIPMENT | ASSIGNED_LANE_DETAIL_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_LANE_ID | Source | SHIPMENT | ASSIGNED_LANE_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_MOT_ID | Source | SHIPMENT | ASSIGNED_MOT_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_SCNDR_CARRIER_CODE | Source | SHIPMENT | ASSIGNED_SCNDR_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_SCNDR_CARRIER_ID | Source | SHIPMENT | ASSIGNED_SCNDR_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_SERVICE_LEVEL_ID | Source | SHIPMENT | ASSIGNED_SERVICE_LEVEL_ID | 1-1 Mapping |
| Bronze | bz_shipment | ASSIGNED_SHIP_VIA | Source | SHIPMENT | ASSIGNED_SHIP_VIA | 1-1 Mapping |
| Bronze | bz_shipment | AUTH_NBR | Source | SHIPMENT | AUTH_NBR | 1-1 Mapping |
| Bronze | bz_shipment | AVAILABLE_DTTM | Source | SHIPMENT | AVAILABLE_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | BASELINE_COST | Source | SHIPMENT | BASELINE_COST | 1-1 Mapping |
| Bronze | bz_shipment | BASELINE_COST_CURRENCY_CODE | Source | SHIPMENT | BASELINE_COST_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | BILL_OF_LADING_NUMBER | Source | SHIPMENT | BILL_OF_LADING_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_ADDRESS | Source | SHIPMENT | BILL_TO_ADDRESS | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_CITY | Source | SHIPMENT | BILL_TO_CITY | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_CODE | Source | SHIPMENT | BILL_TO_CODE | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_COUNTRY_CODE | Source | SHIPMENT | BILL_TO_COUNTRY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_NAME | Source | SHIPMENT | BILL_TO_NAME | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_PHONE_NUMBER | Source | SHIPMENT | BILL_TO_PHONE_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_POSTAL_CODE | Source | SHIPMENT | BILL_TO_POSTAL_CODE | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_STATE_PROV | Source | SHIPMENT | BILL_TO_STATE_PROV | 1-1 Mapping |
| Bronze | bz_shipment | BILL_TO_TITLE | Source | SHIPMENT | BILL_TO_TITLE | 1-1 Mapping |
| Bronze | bz_shipment | BILLING_METHOD | Source | SHIPMENT | BILLING_METHOD | 1-1 Mapping |
| Bronze | bz_shipment | BK_ARRIVAL_DTTM | Source | SHIPMENT | BK_ARRIVAL_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | BK_ARRIVAL_TZ | Source | SHIPMENT | BK_ARRIVAL_TZ | 1-1 Mapping |
| Bronze | bz_shipment | BK_CUTOFF_DTTM | Source | SHIPMENT | BK_CUTOFF_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | BK_CUTOFF_TZ | Source | SHIPMENT | BK_CUTOFF_TZ | 1-1 Mapping |
| Bronze | bz_shipment | BK_D_FACILITY_ALIAS_ID | Source | SHIPMENT | BK_D_FACILITY_ALIAS_ID | 1-1 Mapping |
| Bronze | bz_shipment | BK_D_FACILITY_ID | Source | SHIPMENT | BK_D_FACILITY_ID | 1-1 Mapping |
| Bronze | bz_shipment | BK_DEPARTURE_DTTM | Source | SHIPMENT | BK_DEPARTURE_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | BK_DEPARTURE_TZ | Source | SHIPMENT | BK_DEPARTURE_TZ | 1-1 Mapping |
| Bronze | bz_shipment | BK_FORWARDER_AIRWAY_BILL | Source | SHIPMENT | BK_FORWARDER_AIRWAY_BILL | 1-1 Mapping |
| Bronze | bz_shipment | BK_MASTER_AIRWAY_BILL | Source | SHIPMENT | BK_MASTER_AIRWAY_BILL | 1-1 Mapping |
| Bronze | bz_shipment | BK_O_FACILITY_ALIAS_ID | Source | SHIPMENT | BK_O_FACILITY_ALIAS_ID | 1-1 Mapping |
| Bronze | bz_shipment | BK_O_FACILITY_ID | Source | SHIPMENT | BK_O_FACILITY_ID | 1-1 Mapping |
| Bronze | bz_shipment | BK_PICKUP_DTTM | Source | SHIPMENT | BK_PICKUP_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | BK_PICKUP_TZ | Source | SHIPMENT | BK_PICKUP_TZ | 1-1 Mapping |
| Bronze | bz_shipment | BK_RESOURCE_NAME_EXTERNAL | Source | SHIPMENT | BK_RESOURCE_NAME_EXTERNAL | 1-1 Mapping |
| Bronze | bz_shipment | BK_RESOURCE_REF_EXTERNAL | Source | SHIPMENT | BK_RESOURCE_REF_EXTERNAL | 1-1 Mapping |
| Bronze | bz_shipment | BOOKING_ID | Source | SHIPMENT | BOOKING_ID | 1-1 Mapping |
| Bronze | bz_shipment | BOOKING_REF_CARRIER | Source | SHIPMENT | BOOKING_REF_CARRIER | 1-1 Mapping |
| Bronze | bz_shipment | BOOKING_REF_SHIPPER | Source | SHIPMENT | BOOKING_REF_SHIPPER | 1-1 Mapping |
| Bronze | bz_shipment | BROKER_CARRIER_ID | Source | SHIPMENT | BROKER_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | BROKER_REF | Source | SHIPMENT | BROKER_REF | 1-1 Mapping |
| Bronze | bz_shipment | BUDG_CM_DISCOUNT | Source | SHIPMENT | BUDG_CM_DISCOUNT | 1-1 Mapping |
| Bronze | bz_shipment | BUDG_CURRENCY_CODE | Source | SHIPMENT | BUDG_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | BUDG_NORMALIZED_TOTAL_COST | Source | SHIPMENT | BUDG_NORMALIZED_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | BUDG_TOTAL_COST | Source | SHIPMENT | BUDG_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | BUSINESS_PARTNER_ID | Source | SHIPMENT | BUSINESS_PARTNER_ID | 1-1 Mapping |
| Bronze | bz_shipment | BUSINESS_PROCESS | Source | SHIPMENT | BUSINESS_PROCESS | 1-1 Mapping |
| Bronze | bz_shipment | CARRIER_CHARGE | Source | SHIPMENT | CARRIER_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | CFMF_STATUS | Source | SHIPMENT | CFMF_STATUS | 1-1 Mapping |
| Bronze | bz_shipment | CM_DISCOUNT | Source | SHIPMENT | CM_DISCOUNT | 1-1 Mapping |
| Bronze | bz_shipment | CMID | Source | SHIPMENT | CMID | 1-1 Mapping |
| Bronze | bz_shipment | COD_AMOUNT | Source | SHIPMENT | COD_AMOUNT | 1-1 Mapping |
| Bronze | bz_shipment | COD_CURRENCY_CODE | Source | SHIPMENT | COD_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | COMMODITY_CLASS | Source | SHIPMENT | COMMODITY_CLASS | 1-1 Mapping |
| Bronze | bz_shipment | COMMODITY_CODE_ID | Source | SHIPMENT | COMMODITY_CODE_ID | 1-1 Mapping |
| Bronze | bz_shipment | CONFIG_CYCLE_SEQ | Source | SHIPMENT | CONFIG_CYCLE_SEQ | 1-1 Mapping |
| Bronze | bz_shipment | CONS_ADDR_CODE | Source | SHIPMENT | CONS_ADDR_CODE | 1-1 Mapping |
| Bronze | bz_shipment | CONS_LOCN_ID | Source | SHIPMENT | CONS_LOCN_ID | 1-1 Mapping |
| Bronze | bz_shipment | CONS_RUN_ID | Source | SHIPMENT | CONS_RUN_ID | 1-1 Mapping |
| Bronze | bz_shipment | CONTRACT_NUMBER | Source | SHIPMENT | CONTRACT_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | COST_BREAKUP | Source | SHIPMENT | COST_BREAKUP | 1-1 Mapping |
| Bronze | bz_shipment | CREATED_DTTM | Source | SHIPMENT | CREATED_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | CREATED_SOURCE | Source | SHIPMENT | CREATED_SOURCE | 1-1 Mapping |
| Bronze | bz_shipment | CREATED_SOURCE_TYPE | Source | SHIPMENT | CREATED_SOURCE_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | CREATION_TYPE | Source | SHIPMENT | CREATION_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | CURRENCY_CODE | Source | SHIPMENT | CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | CURRENCY_DTTM | Source | SHIPMENT | CURRENCY_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | CUST_FRGT_CHARGE | Source | SHIPMENT | CUST_FRGT_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | CUSTOMER_CREDIT_LIMIT_ID | Source | SHIPMENT | CUSTOMER_CREDIT_LIMIT_ID | 1-1 Mapping |
| Bronze | bz_shipment | CUSTOMER_ID | Source | SHIPMENT | CUSTOMER_ID | 1-1 Mapping |
| Bronze | bz_shipment | CYCLE_DEADLINE_DTTM | Source | SHIPMENT | CYCLE_DEADLINE_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | CYCLE_EXECUTION_DTTM | Source | SHIPMENT | CYCLE_EXECUTION_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | CYCLE_RESP_DEADLINE_TZ | Source | SHIPMENT | CYCLE_RESP_DEADLINE_TZ | 1-1 Mapping |
| Bronze | bz_shipment | D_ADDRESS | Source | SHIPMENT | D_ADDRESS | 1-1 Mapping |
| Bronze | bz_shipment | D_CITY | Source | SHIPMENT | D_CITY | 1-1 Mapping |
| Bronze | bz_shipment | D_COUNTRY_CODE | Source | SHIPMENT | D_COUNTRY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | D_COUNTY | Source | SHIPMENT | D_COUNTY | 1-1 Mapping |
| Bronze | bz_shipment | D_FACILITY_ID | Source | SHIPMENT | D_FACILITY_ID | 1-1 Mapping |
| Bronze | bz_shipment | D_FACILITY_NUMBER | Source | SHIPMENT | D_FACILITY_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | D_POSTAL_CODE | Source | SHIPMENT | D_POSTAL_CODE | 1-1 Mapping |
| Bronze | bz_shipment | D_STATE_PROV | Source | SHIPMENT | D_STATE_PROV | 1-1 Mapping |
| Bronze | bz_shipment | D_STOP_LOCATION_NAME | Source | SHIPMENT | D_STOP_LOCATION_NAME | 1-1 Mapping |
| Bronze | bz_shipment | D_TANDEM_FACILITY | Source | SHIPMENT | D_TANDEM_FACILITY | 1-1 Mapping |
| Bronze | bz_shipment | D_TANDEM_FACILITY_ALIAS | Source | SHIPMENT | D_TANDEM_FACILITY_ALIAS | 1-1 Mapping |
| Bronze | bz_shipment | DAYS_TO_DELIVER | Source | SHIPMENT | DAYS_TO_DELIVER | 1-1 Mapping |
| Bronze | bz_shipment | DECLARED_VALUE | Source | SHIPMENT | DECLARED_VALUE | 1-1 Mapping |
| Bronze | bz_shipment | DELAY_TYPE | Source | SHIPMENT | DELAY_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | DELIVERY_END_DTTM | Source | SHIPMENT | DELIVERY_END_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | DELIVERY_REQ | Source | SHIPMENT | DELIVERY_REQ | 1-1 Mapping |
| Bronze | bz_shipment | DELIVERY_START_DTTM | Source | SHIPMENT | DELIVERY_START_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | DELIVERY_TZ | Source | SHIPMENT | DELIVERY_TZ | 1-1 Mapping |
| Bronze | bz_shipment | DESIGNATED_DRIVER_TYPE | Source | SHIPMENT | DESIGNATED_DRIVER_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | DESIGNATED_TRACTOR_CODE | Source | SHIPMENT | DESIGNATED_TRACTOR_CODE | 1-1 Mapping |
| Bronze | bz_shipment | DIRECT_DISTANCE | Source | SHIPMENT | DIRECT_DISTANCE | 1-1 Mapping |
| Bronze | bz_shipment | DISTANCE | Source | SHIPMENT | DISTANCE | 1-1 Mapping |
| Bronze | bz_shipment | DISTANCE_UOM | Source | SHIPMENT | DISTANCE_UOM | 1-1 Mapping |
| Bronze | bz_shipment | DOOR | Source | SHIPMENT | DOOR | 1-1 Mapping |
| Bronze | bz_shipment | DRIVER_TYPE_ID | Source | SHIPMENT | DRIVER_TYPE_ID | 1-1 Mapping |
| Bronze | bz_shipment | DROPOFF_PICKUP | Source | SHIPMENT | DROPOFF_PICKUP | 1-1 Mapping |
| Bronze | bz_shipment | DSG_CARRIER_CODE | Source | SHIPMENT | DSG_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | DSG_CARRIER_ID | Source | SHIPMENT | DSG_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | DSG_EQUIPMENT_ID | Source | SHIPMENT | DSG_EQUIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | DSG_MOT_ID | Source | SHIPMENT | DSG_MOT_ID | 1-1 Mapping |
| Bronze | bz_shipment | DSG_SCNDR_CARRIER_CODE | Source | SHIPMENT | DSG_SCNDR_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | DSG_SCNDR_CARRIER_ID | Source | SHIPMENT | DSG_SCNDR_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | DSG_SERVICE_LEVEL_ID | Source | SHIPMENT | DSG_SERVICE_LEVEL_ID | 1-1 Mapping |
| Bronze | bz_shipment | DSG_VOYAGE_FLIGHT | Source | SHIPMENT | DSG_VOYAGE_FLIGHT | 1-1 Mapping |
| Bronze | bz_shipment | DT_PARAM_SET_ID | Source | SHIPMENT | DT_PARAM_SET_ID | 1-1 Mapping |
| Bronze | bz_shipment | DV_CURRENCY_CODE | Source | SHIPMENT | DV_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | EARNED_INCOME | Source | SHIPMENT | EARNED_INCOME | 1-1 Mapping |
| Bronze | bz_shipment | EARNED_INCOME_CURRENCY_CODE | Source | SHIPMENT | EARNED_INCOME_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | EQUIP_UTIL_PER | Source | SHIPMENT | EQUIP_UTIL_PER | 1-1 Mapping |
| Bronze | bz_shipment | EQUIPMENT_TYPE | Source | SHIPMENT | EQUIPMENT_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | ESTIMATED_COST | Source | SHIPMENT | ESTIMATED_COST | 1-1 Mapping |
| Bronze | bz_shipment | ESTIMATED_DISPATCH_DTTM | Source | SHIPMENT | ESTIMATED_DISPATCH_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | ESTIMATED_SAVINGS | Source | SHIPMENT | ESTIMATED_SAVINGS | 1-1 Mapping |
| Bronze | bz_shipment | EVENT_IND_TYPEID | Source | SHIPMENT | EVENT_IND_TYPEID | 1-1 Mapping |
| Bronze | bz_shipment | EXT_SYS_SHIPMENT_ID | Source | SHIPMENT | EXT_SYS_SHIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | EXTRACTION_DTTM | Source | SHIPMENT | EXTRACTION_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | FACILITY_SCHEDULE_ID | Source | SHIPMENT | FACILITY_SCHEDULE_ID | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_CARRIER_CODE | Source | SHIPMENT | FEASIBLE_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_CARRIER_ID | Source | SHIPMENT | FEASIBLE_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_DRIVER_TYPE | Source | SHIPMENT | FEASIBLE_DRIVER_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_EQUIPMENT_ID | Source | SHIPMENT | FEASIBLE_EQUIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_EQUIPMENT2_ID | Source | SHIPMENT | FEASIBLE_EQUIPMENT2_ID | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_MOT_ID | Source | SHIPMENT | FEASIBLE_MOT_ID | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_SERVICE_LEVEL_ID | Source | SHIPMENT | FEASIBLE_SERVICE_LEVEL_ID | 1-1 Mapping |
| Bronze | bz_shipment | FEASIBLE_VOYAGE_FLIGHT | Source | SHIPMENT | FEASIBLE_VOYAGE_FLIGHT | 1-1 Mapping |
| Bronze | bz_shipment | FINANCIAL_WT | Source | SHIPMENT | FINANCIAL_WT | 1-1 Mapping |
| Bronze | bz_shipment | FIRST_UPDATE_SENT_TO_PKMS | Source | SHIPMENT | FIRST_UPDATE_SENT_TO_PKMS | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_ACCESSORIAL_CHARGE | Source | SHIPMENT | FRT_REV_ACCESSORIAL_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_CM_DISCOUNT | Source | SHIPMENT | FRT_REV_CM_DISCOUNT | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_LINEHAUL_CHARGE | Source | SHIPMENT | FRT_REV_LINEHAUL_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_RATING_LANE_DETAIL_ID | Source | SHIPMENT | FRT_REV_RATING_LANE_DETAIL_ID | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_RATING_LANE_ID | Source | SHIPMENT | FRT_REV_RATING_LANE_ID | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_SPOT_CHARGE | Source | SHIPMENT | FRT_REV_SPOT_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_SPOT_CHARGE_CURR_CODE | Source | SHIPMENT | FRT_REV_SPOT_CHARGE_CURR_CODE | 1-1 Mapping |
| Bronze | bz_shipment | FRT_REV_STOP_CHARGE | Source | SHIPMENT | FRT_REV_STOP_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | GRS_MAX_SHIPMENT_STATUS | Source | SHIPMENT | GRS_MAX_SHIPMENT_STATUS | 1-1 Mapping |
| Bronze | bz_shipment | HAS_ALERTS | Source | SHIPMENT | HAS_ALERTS | 1-1 Mapping |
| Bronze | bz_shipment | HAS_EM_NOTIFY_FLAG | Source | SHIPMENT | HAS_EM_NOTIFY_FLAG | 1-1 Mapping |
| Bronze | bz_shipment | HAS_IMPORT_ERROR | Source | SHIPMENT | HAS_IMPORT_ERROR | 1-1 Mapping |
| Bronze | bz_shipment | HAS_NOTES | Source | SHIPMENT | HAS_NOTES | 1-1 Mapping |
| Bronze | bz_shipment | HAS_SOFT_CHECK_ERROR | Source | SHIPMENT | HAS_SOFT_CHECK_ERROR | 1-1 Mapping |
| Bronze | bz_shipment | HAS_TRACKING_MSG | Source | SHIPMENT | HAS_TRACKING_MSG | 1-1 Mapping |
| Bronze | bz_shipment | HAULING_CARRIER | Source | SHIPMENT | HAULING_CARRIER | 1-1 Mapping |
| Bronze | bz_shipment | HAZMAT_CERT_CONTACT | Source | SHIPMENT | HAZMAT_CERT_CONTACT | 1-1 Mapping |
| Bronze | bz_shipment | HAZMAT_CERT_DECLARATION | Source | SHIPMENT | HAZMAT_CERT_DECLARATION | 1-1 Mapping |
| Bronze | bz_shipment | HIBERNATE_VERSION | Source | SHIPMENT | HIBERNATE_VERSION | 1-1 Mapping |
| Bronze | bz_shipment | HUB_ID | Source | SHIPMENT | HUB_ID | 1-1 Mapping |
| Bronze | bz_shipment | INBOUND_REGION_ID | Source | SHIPMENT | INBOUND_REGION_ID | 1-1 Mapping |
| Bronze | bz_shipment | INCOTERM_ID | Source | SHIPMENT | INCOTERM_ID | 1-1 Mapping |
| Bronze | bz_shipment | INSURANCE_STATUS | Source | SHIPMENT | INSURANCE_STATUS | 1-1 Mapping |
| Bronze | bz_shipment | IS_ASSOCIATED_TO_OUTBOUND | Source | SHIPMENT | IS_ASSOCIATED_TO_OUTBOUND | 1-1 Mapping |
| Bronze | bz_shipment | IS_AUTO_DELIVERED | Source | SHIPMENT | IS_AUTO_DELIVERED | 1-1 Mapping |
| Bronze | bz_shipment | IS_BOOKING_REQUIRED | Source | SHIPMENT | IS_BOOKING_REQUIRED | 1-1 Mapping |
| Bronze | bz_shipment | IS_CM_OPTION_GEN_ACTIVE | Source | SHIPMENT | IS_CM_OPTION_GEN_ACTIVE | 1-1 Mapping |
| Bronze | bz_shipment | IS_COOLER_AT_NOSE | Source | SHIPMENT | IS_COOLER_AT_NOSE | 1-1 Mapping |
| Bronze | bz_shipment | IS_FILO | Source | SHIPMENT | IS_FILO | 1-1 Mapping |
| Bronze | bz_shipment | IS_GRS_OPT_CYCLE_RUNNING | Source | SHIPMENT | IS_GRS_OPT_CYCLE_RUNNING | 1-1 Mapping |
| Bronze | bz_shipment | IS_HAZMAT | Source | SHIPMENT | IS_HAZMAT | 1-1 Mapping |
| Bronze | bz_shipment | IS_MANUAL_ASSIGN | Source | SHIPMENT | IS_MANUAL_ASSIGN | 1-1 Mapping |
| Bronze | bz_shipment | IS_MISROUTED | Source | SHIPMENT | IS_MISROUTED | 1-1 Mapping |
| Bronze | bz_shipment | IS_PERISHABLE | Source | SHIPMENT | IS_PERISHABLE | 1-1 Mapping |
| Bronze | bz_shipment | IS_SHIPMENT_CANCELLED | Source | SHIPMENT | IS_SHIPMENT_CANCELLED | 1-1 Mapping |
| Bronze | bz_shipment | IS_SHIPMENT_RECONCILED | Source | SHIPMENT | IS_SHIPMENT_RECONCILED | 1-1 Mapping |
| Bronze | bz_shipment | IS_TIME_FEAS_ENABLED | Source | SHIPMENT | IS_TIME_FEAS_ENABLED | 1-1 Mapping |
| Bronze | bz_shipment | IS_WAVE_MAN_CHANGED | Source | SHIPMENT | IS_WAVE_MAN_CHANGED | 1-1 Mapping |
| Bronze | bz_shipment | LANE_NAME | Source | SHIPMENT | LANE_NAME | 1-1 Mapping |
| Bronze | bz_shipment | LAST_CM_OPTION_GEN_DTTM | Source | SHIPMENT | LAST_CM_OPTION_GEN_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | LAST_RS_NOTIFICATION_DTTM | Source | SHIPMENT | LAST_RS_NOTIFICATION_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | LAST_RUN_GRS_DTTM | Source | SHIPMENT | LAST_RUN_GRS_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | LAST_SELECTOR_RUN_DTTM | Source | SHIPMENT | LAST_SELECTOR_RUN_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | LAST_UPDATED_DTTM | Source | SHIPMENT | LAST_UPDATED_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | LAST_UPDATED_SOURCE | Source | SHIPMENT | LAST_UPDATED_SOURCE | 1-1 Mapping |
| Bronze | bz_shipment | LAST_UPDATED_SOURCE_TYPE | Source | SHIPMENT | LAST_UPDATED_SOURCE_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | LEFT_WT | Source | SHIPMENT | LEFT_WT | 1-1 Mapping |
| Bronze | bz_shipment | LH_PAYEE_CARRIER_CODE | Source | SHIPMENT | LH_PAYEE_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | LH_PAYEE_CARRIER_ID | Source | SHIPMENT | LH_PAYEE_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | LINEHAUL_COST | Source | SHIPMENT | LINEHAUL_COST | 1-1 Mapping |
| Bronze | bz_shipment | LOADING_SEQ_ORD | Source | SHIPMENT | LOADING_SEQ_ORD | 1-1 Mapping |
| Bronze | bz_shipment | LOC_REFERENCE | Source | SHIPMENT | LOC_REFERENCE | 1-1 Mapping |
| Bronze | bz_shipment | LPN_ASSIGNMENT_STOPPED | Source | SHIPMENT | LPN_ASSIGNMENT_STOPPED | 1-1 Mapping |
| Bronze | bz_shipment | MANIFEST_ID | Source | SHIPMENT | MANIFEST_ID | 1-1 Mapping |
| Bronze | bz_shipment | MARGIN | Source | SHIPMENT | MARGIN | 1-1 Mapping |
| Bronze | bz_shipment | MAX_NBR_OF_CTNS | Source | SHIPMENT | MAX_NBR_OF_CTNS | 1-1 Mapping |
| Bronze | bz_shipment | MERCHANDIZING_DEPARTMENT_ID | Source | SHIPMENT | MERCHANDIZING_DEPARTMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | MIN_RATE | Source | SHIPMENT | MIN_RATE | 1-1 Mapping |
| Bronze | bz_shipment | MONETARY_VALUE | Source | SHIPMENT | MONETARY_VALUE | 1-1 Mapping |
| Bronze | bz_shipment | MOVE_TYPE | Source | SHIPMENT | MOVE_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | MV_CURRENCY_CODE | Source | SHIPMENT | MV_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | NORM_SPOT_CHARGE_AND_PAYEE_ACC | Source | SHIPMENT | NORM_SPOT_CHARGE_AND_PAYEE_ACC | 1-1 Mapping |
| Bronze | bz_shipment | NORMALIZED_BASELINE_COST | Source | SHIPMENT | NORMALIZED_BASELINE_COST | 1-1 Mapping |
| Bronze | bz_shipment | NORMALIZED_MARGIN | Source | SHIPMENT | NORMALIZED_MARGIN | 1-1 Mapping |
| Bronze | bz_shipment | NORMALIZED_TOTAL_COST | Source | SHIPMENT | NORMALIZED_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | NORMALIZED_TOTAL_REVENUE | Source | SHIPMENT | NORMALIZED_TOTAL_REVENUE | 1-1 Mapping |
| Bronze | bz_shipment | NUM_CHARGE_LAYOVERS | Source | SHIPMENT | NUM_CHARGE_LAYOVERS | 1-1 Mapping |
| Bronze | bz_shipment | NUM_DOCKS | Source | SHIPMENT | NUM_DOCKS | 1-1 Mapping |
| Bronze | bz_shipment | NUM_STOPS | Source | SHIPMENT | NUM_STOPS | 1-1 Mapping |
| Bronze | bz_shipment | O_ADDRESS | Source | SHIPMENT | O_ADDRESS | 1-1 Mapping |
| Bronze | bz_shipment | O_CITY | Source | SHIPMENT | O_CITY | 1-1 Mapping |
| Bronze | bz_shipment | O_COUNTRY_CODE | Source | SHIPMENT | O_COUNTRY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | O_COUNTY | Source | SHIPMENT | O_COUNTY | 1-1 Mapping |
| Bronze | bz_shipment | O_FACILITY_ID | Source | SHIPMENT | O_FACILITY_ID | 1-1 Mapping |
| Bronze | bz_shipment | O_FACILITY_NUMBER | Source | SHIPMENT | O_FACILITY_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | O_POSTAL_CODE | Source | SHIPMENT | O_POSTAL_CODE | 1-1 Mapping |
| Bronze | bz_shipment | O_STATE_PROV | Source | SHIPMENT | O_STATE_PROV | 1-1 Mapping |
| Bronze | bz_shipment | O_STOP_LOCATION_NAME | Source | SHIPMENT | O_STOP_LOCATION_NAME | 1-1 Mapping |
| Bronze | bz_shipment | O_TANDEM_FACILITY | Source | SHIPMENT | O_TANDEM_FACILITY | 1-1 Mapping |
| Bronze | bz_shipment | O_TANDEM_FACILITY_ALIAS | Source | SHIPMENT | O_TANDEM_FACILITY_ALIAS | 1-1 Mapping |
| Bronze | bz_shipment | OCEAN_ROUTING_STAGE | Source | SHIPMENT | OCEAN_ROUTING_STAGE | 1-1 Mapping |
| Bronze | bz_shipment | ON_TIME_INDICATOR | Source | SHIPMENT | ON_TIME_INDICATOR | 1-1 Mapping |
| Bronze | bz_shipment | ORDER_QTY | Source | SHIPMENT | ORDER_QTY | 1-1 Mapping |
| Bronze | bz_shipment | ORIG_BUDG_TOTAL_COST | Source | SHIPMENT | ORIG_BUDG_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | OUT_OF_ROUTE_DISTANCE | Source | SHIPMENT | OUT_OF_ROUTE_DISTANCE | 1-1 Mapping |
| Bronze | bz_shipment | OUTBOUND_REGION_ID | Source | SHIPMENT | OUTBOUND_REGION_ID | 1-1 Mapping |
| Bronze | bz_shipment | PACKAGING | Source | SHIPMENT | PACKAGING | 1-1 Mapping |
| Bronze | bz_shipment | PAPERWORK_START_DTTM | Source | SHIPMENT | PAPERWORK_START_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | PAYEE_CARRIER_ID | Source | SHIPMENT | PAYEE_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | PICK_START_DATE | Source | SHIPMENT | PICK_START_DATE | 1-1 Mapping |
| Bronze | bz_shipment | PICKUP_END_DTTM | Source | SHIPMENT | PICKUP_END_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | PICKUP_START_DATE | Source | SHIPMENT | PICKUP_START_DATE | 1-1 Mapping |
| Bronze | bz_shipment | PICKUP_TZ | Source | SHIPMENT | PICKUP_TZ | 1-1 Mapping |
| Bronze | bz_shipment | PLANNED_VOLUME | Source | SHIPMENT | PLANNED_VOLUME | 1-1 Mapping |
| Bronze | bz_shipment | PLANNED_WEIGHT | Source | SHIPMENT | PLANNED_WEIGHT | 1-1 Mapping |
| Bronze | bz_shipment | PLN_ACCESSORL_COST_TO_CARRIER | Source | SHIPMENT | PLN_ACCESSORL_COST_TO_CARRIER | 1-1 Mapping |
| Bronze | bz_shipment | PLN_CARRIER_CHARGE | Source | SHIPMENT | PLN_CARRIER_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | PLN_CURRENCY_CODE | Source | SHIPMENT | PLN_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | PLN_LINEHAUL_COST | Source | SHIPMENT | PLN_LINEHAUL_COST | 1-1 Mapping |
| Bronze | bz_shipment | PLN_MAX_TEMPERATURE | Source | SHIPMENT | PLN_MAX_TEMPERATURE | 1-1 Mapping |
| Bronze | bz_shipment | PLN_MIN_TEMPERATURE | Source | SHIPMENT | PLN_MIN_TEMPERATURE | 1-1 Mapping |
| Bronze | bz_shipment | PLN_NORMALIZED_TOTAL_COST | Source | SHIPMENT | PLN_NORMALIZED_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | PLN_RATING_LANE_DETAIL_ID | Source | SHIPMENT | PLN_RATING_LANE_DETAIL_ID | 1-1 Mapping |
| Bronze | bz_shipment | PLN_RATING_LANE_ID | Source | SHIPMENT | PLN_RATING_LANE_ID | 1-1 Mapping |
| Bronze | bz_shipment | PLN_STOP_OFF_COST | Source | SHIPMENT | PLN_STOP_OFF_COST | 1-1 Mapping |
| Bronze | bz_shipment | PLN_TOTAL_ACCESSORIAL_COST | Source | SHIPMENT | PLN_TOTAL_ACCESSORIAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | PLN_TOTAL_COST | Source | SHIPMENT | PLN_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | PP_SHIPMENT_ID | Source | SHIPMENT | PP_SHIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | PRINT_CONS_BOL | Source | SHIPMENT | PRINT_CONS_BOL | 1-1 Mapping |
| Bronze | bz_shipment | PRIORITY_TYPE | Source | SHIPMENT | PRIORITY_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | PRO_NUMBER | Source | SHIPMENT | PRO_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | PROD_SCHED_REF_NUMBER | Source | SHIPMENT | PROD_SCHED_REF_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | PRODUCT_CLASS_ID | Source | SHIPMENT | PRODUCT_CLASS_ID | 1-1 Mapping |
| Bronze | bz_shipment | PROTECTION_LEVEL_ID | Source | SHIPMENT | PROTECTION_LEVEL_ID | 1-1 Mapping |
| Bronze | bz_shipment | PURCHASE_ORDER | Source | SHIPMENT | PURCHASE_ORDER | 1-1 Mapping |
| Bronze | bz_shipment | QTY_UOM_ID | Source | SHIPMENT | QTY_UOM_ID | 1-1 Mapping |
| Bronze | bz_shipment | RADIAL_DISTANCE | Source | SHIPMENT | RADIAL_DISTANCE | 1-1 Mapping |
| Bronze | bz_shipment | RADIAL_DISTANCE_UOM | Source | SHIPMENT | RADIAL_DISTANCE_UOM | 1-1 Mapping |
| Bronze | bz_shipment | RATE | Source | SHIPMENT | RATE | 1-1 Mapping |
| Bronze | bz_shipment | RATE_TYPE | Source | SHIPMENT | RATE_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | RATE_UOM | Source | SHIPMENT | RATE_UOM | 1-1 Mapping |
| Bronze | bz_shipment | RATING_LANE_DETAIL_ID | Source | SHIPMENT | RATING_LANE_DETAIL_ID | 1-1 Mapping |
| Bronze | bz_shipment | RATING_LANE_ID | Source | SHIPMENT | RATING_LANE_ID | 1-1 Mapping |
| Bronze | bz_shipment | RATING_QUALIFIER | Source | SHIPMENT | RATING_QUALIFIER | 1-1 Mapping |
| Bronze | bz_shipment | REC_ACCESSORIAL_COST | Source | SHIPMENT | REC_ACCESSORIAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_BROKER_CARRIER_CODE | Source | SHIPMENT | REC_BROKER_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | REC_BROKER_CARRIER_ID | Source | SHIPMENT | REC_BROKER_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_ACCESSORIAL_COST | Source | SHIPMENT | REC_BUDG_ACCESSORIAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_CM_DISCOUNT | Source | SHIPMENT | REC_BUDG_CM_DISCOUNT | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_CURRENCY_CODE | Source | SHIPMENT | REC_BUDG_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_LINEHAUL_COST | Source | SHIPMENT | REC_BUDG_LINEHAUL_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_NORMALIZED_TOTAL_COST | Source | SHIPMENT | REC_BUDG_NORMALIZED_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_RATING_LANE_DETAIL_ID | Source | SHIPMENT | REC_BUDG_RATING_LANE_DETAIL_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_RATING_LANE_ID | Source | SHIPMENT | REC_BUDG_RATING_LANE_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_STOP_COST | Source | SHIPMENT | REC_BUDG_STOP_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_BUDG_TOTAL_COST | Source | SHIPMENT | REC_BUDG_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_CARRIER_CODE | Source | SHIPMENT | REC_CARRIER_CODE | 1-1 Mapping |
| Bronze | bz_shipment | REC_CARRIER_ID | Source | SHIPMENT | REC_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_CM_DISCOUNT | Source | SHIPMENT | REC_CM_DISCOUNT | 1-1 Mapping |
| Bronze | bz_shipment | REC_CM_SHIPMENT_ID | Source | SHIPMENT | REC_CM_SHIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_CMID | Source | SHIPMENT | REC_CMID | 1-1 Mapping |
| Bronze | bz_shipment | REC_COST_BREAKUP | Source | SHIPMENT | REC_COST_BREAKUP | 1-1 Mapping |
| Bronze | bz_shipment | REC_CURRENCY_CODE | Source | SHIPMENT | REC_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | REC_EQUIPMENT_ID | Source | SHIPMENT | REC_EQUIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_LANE_DETAIL_ID | Source | SHIPMENT | REC_LANE_DETAIL_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_LANE_ID | Source | SHIPMENT | REC_LANE_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_LINEHAUL_COST | Source | SHIPMENT | REC_LINEHAUL_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_MARGIN | Source | SHIPMENT | REC_MARGIN | 1-1 Mapping |
| Bronze | bz_shipment | REC_MOT_ID | Source | SHIPMENT | REC_MOT_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_NORMALIZED_MARGIN | Source | SHIPMENT | REC_NORMALIZED_MARGIN | 1-1 Mapping |
| Bronze | bz_shipment | REC_NORMALIZED_TOTAL_COST | Source | SHIPMENT | REC_NORMALIZED_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_RATING_LANE_DETAIL_ID | Source | SHIPMENT | REC_RATING_LANE_DETAIL_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_RATING_LANE_ID | Source | SHIPMENT | REC_RATING_LANE_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_SERVICE_LEVEL_ID | Source | SHIPMENT | REC_SERVICE_LEVEL_ID | 1-1 Mapping |
| Bronze | bz_shipment | REC_SPOT_CHARGE | Source | SHIPMENT | REC_SPOT_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | REC_SPOT_CHARGE_CURRENCY_CODE | Source | SHIPMENT | REC_SPOT_CHARGE_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | REC_STOP_COST | Source | SHIPMENT | REC_STOP_COST | 1-1 Mapping |
| Bronze | bz_shipment | REC_TOTAL_COST | Source | SHIPMENT | REC_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | RECEIVED_DTTM | Source | SHIPMENT | RECEIVED_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | REF_SHIPMENT_NBR | Source | SHIPMENT | REF_SHIPMENT_NBR | 1-1 Mapping |
| Bronze | bz_shipment | REGION_ID | Source | SHIPMENT | REGION_ID | 1-1 Mapping |
| Bronze | bz_shipment | REPORTED_COST | Source | SHIPMENT | REPORTED_COST | 1-1 Mapping |
| Bronze | bz_shipment | RETAIN_CONSOLIDATOR_TIMES | Source | SHIPMENT | RETAIN_CONSOLIDATOR_TIMES | 1-1 Mapping |
| Bronze | bz_shipment | REVENUE_RATING_LEVEL | Source | SHIPMENT | REVENUE_RATING_LEVEL | 1-1 Mapping |
| Bronze | bz_shipment | RIGHT_WT | Source | SHIPMENT | RIGHT_WT | 1-1 Mapping |
| Bronze | bz_shipment | RS_AREA_ID | Source | SHIPMENT | RS_AREA_ID | 1-1 Mapping |
| Bronze | bz_shipment | RS_CONFIG_CYCLE_ID | Source | SHIPMENT | RS_CONFIG_CYCLE_ID | 1-1 Mapping |
| Bronze | bz_shipment | RS_CONFIG_ID | Source | SHIPMENT | RS_CONFIG_ID | 1-1 Mapping |
| Bronze | bz_shipment | RS_CYCLE_REMAINING | Source | SHIPMENT | RS_CYCLE_REMAINING | 1-1 Mapping |
| Bronze | bz_shipment | RS_TYPE | Source | SHIPMENT | RS_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | RTE_SWC_NBR | Source | SHIPMENT | RTE_SWC_NBR | 1-1 Mapping |
| Bronze | bz_shipment | RTE_TO | Source | SHIPMENT | RTE_TO | 1-1 Mapping |
| Bronze | bz_shipment | RTE_TYPE | Source | SHIPMENT | RTE_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | RTE_TYPE_1 | Source | SHIPMENT | RTE_TYPE_1 | 1-1 Mapping |
| Bronze | bz_shipment | RTE_TYPE_2 | Source | SHIPMENT | RTE_TYPE_2 | 1-1 Mapping |
| Bronze | bz_shipment | SCHEDULED_PICKUP_DTTM | Source | SHIPMENT | SCHEDULED_PICKUP_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | SCNDR_CARRIER_ID | Source | SHIPMENT | SCNDR_CARRIER_ID | 1-1 Mapping |
| Bronze | bz_shipment | SEAL_NUMBER | Source | SHIPMENT | SEAL_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | SED_GENERATED_FLAG | Source | SHIPMENT | SED_GENERATED_FLAG | 1-1 Mapping |
| Bronze | bz_shipment | SENT_TO_CREATE_PKMS | Source | SHIPMENT | SENT_TO_CREATE_PKMS | 1-1 Mapping |
| Bronze | bz_shipment | SENT_TO_CREATE_PKMS_DTTM | Source | SHIPMENT | SENT_TO_CREATE_PKMS_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | SENT_TO_PKMS | Source | SHIPMENT | SENT_TO_PKMS | 1-1 Mapping |
| Bronze | bz_shipment | SENT_TO_PKMS_DTTM | Source | SHIPMENT | SENT_TO_PKMS_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | SERV_AREA_CODE | Source | SHIPMENT | SERV_AREA_CODE | 1-1 Mapping |
| Bronze | bz_shipment | SHIP_GROUP_ID | Source | SHIPMENT | SHIP_GROUP_ID | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_CLOSED_INDICATOR | Source | SHIPMENT | SHIPMENT_CLOSED_INDICATOR | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_END_DTTM | Source | SHIPMENT | SHIPMENT_END_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_LEG_TYPE | Source | SHIPMENT | SHIPMENT_LEG_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_RECON_DTTM | Source | SHIPMENT | SHIPMENT_RECON_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_REF_ID | Source | SHIPMENT | SHIPMENT_REF_ID | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_START_DTTM | Source | SHIPMENT | SHIPMENT_START_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_STATUS | Source | SHIPMENT | SHIPMENT_STATUS | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_TYPE | Source | SHIPMENT | SHIPMENT_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | SHIPMENT_WIN_ADJ_FLAG | Source | SHIPMENT | SHIPMENT_WIN_ADJ_FLAG | 1-1 Mapping |
| Bronze | bz_shipment | SIZE1_UOM_ID | Source | SHIPMENT | SIZE1_UOM_ID | 1-1 Mapping |
| Bronze | bz_shipment | SIZE1_VALUE | Source | SHIPMENT | SIZE1_VALUE | 1-1 Mapping |
| Bronze | bz_shipment | SIZE2_UOM_ID | Source | SHIPMENT | SIZE2_UOM_ID | 1-1 Mapping |
| Bronze | bz_shipment | SIZE2_VALUE | Source | SHIPMENT | SIZE2_VALUE | 1-1 Mapping |
| Bronze | bz_shipment | SPOT_CHARGE | Source | SHIPMENT | SPOT_CHARGE | 1-1 Mapping |
| Bronze | bz_shipment | SPOT_CHARGE_AND_PAYEE_ACC | Source | SHIPMENT | SPOT_CHARGE_AND_PAYEE_ACC | 1-1 Mapping |
| Bronze | bz_shipment | SPOT_CHARGE_AND_PAYEE_ACC_CC | Source | SHIPMENT | SPOT_CHARGE_AND_PAYEE_ACC_CC | 1-1 Mapping |
| Bronze | bz_shipment | SPOT_CHARGE_CURRENCY_CODE | Source | SHIPMENT | SPOT_CHARGE_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | STAGING_LOCN_ID | Source | SHIPMENT | STAGING_LOCN_ID | 1-1 Mapping |
| Bronze | bz_shipment | STATIC_ROUTE_ID | Source | SHIPMENT | STATIC_ROUTE_ID | 1-1 Mapping |
| Bronze | bz_shipment | STATUS_CHANGE_DATE | Source | SHIPMENT | STATUS_CHANGE_DATE | 1-1 Mapping |
| Bronze | bz_shipment | STOP_COST | Source | SHIPMENT | STOP_COST | 1-1 Mapping |
| Bronze | bz_shipment | TANDEM_PATH_ID | Source | SHIPMENT | TANDEM_PATH_ID | 1-1 Mapping |
| Bronze | bz_shipment | TARIFF | Source | SHIPMENT | TARIFF | 1-1 Mapping |
| Bronze | bz_shipment | TC_COMPANY_ID | Source | SHIPMENT | TC_COMPANY_ID | 1-1 Mapping |
| Bronze | bz_shipment | TC_SHIPMENT_ID | Source | SHIPMENT | TC_SHIPMENT_ID | 1-1 Mapping |
| Bronze | bz_shipment | TEMPERATURE_UOM | Source | SHIPMENT | TEMPERATURE_UOM | 1-1 Mapping |
| Bronze | bz_shipment | TENDER_DTTM | Source | SHIPMENT | TENDER_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | TENDER_RESP_DEADLINE_DATE | Source | SHIPMENT | TENDER_RESP_DEADLINE_DATE | 1-1 Mapping |
| Bronze | bz_shipment | TENDER_RESP_DEADLINE_TZ | Source | SHIPMENT | TENDER_RESP_DEADLINE_TZ | 1-1 Mapping |
| Bronze | bz_shipment | TOTAL_COST | Source | SHIPMENT | TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | TOTAL_COST_EXCL_TAX | Source | SHIPMENT | TOTAL_COST_EXCL_TAX | 1-1 Mapping |
| Bronze | bz_shipment | TOTAL_REVENUE | Source | SHIPMENT | TOTAL_REVENUE | 1-1 Mapping |
| Bronze | bz_shipment | TOTAL_REVENUE_CURRENCY_CODE | Source | SHIPMENT | TOTAL_REVENUE_CURRENCY_CODE | 1-1 Mapping |
| Bronze | bz_shipment | TOTAL_TAX_AMOUNT | Source | SHIPMENT | TOTAL_TAX_AMOUNT | 1-1 Mapping |
| Bronze | bz_shipment | TOTAL_TIME | Source | SHIPMENT | TOTAL_TIME | 1-1 Mapping |
| Bronze | bz_shipment | TRACKING_MSG_PROBLEM | Source | SHIPMENT | TRACKING_MSG_PROBLEM | 1-1 Mapping |
| Bronze | bz_shipment | TRACTOR_NUMBER | Source | SHIPMENT | TRACTOR_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | TRAILER_NUMBER | Source | SHIPMENT | TRAILER_NUMBER | 1-1 Mapping |
| Bronze | bz_shipment | TRANS_PLAN_OWNER | Source | SHIPMENT | TRANS_PLAN_OWNER | 1-1 Mapping |
| Bronze | bz_shipment | TRANS_RESP_CODE | Source | SHIPMENT | TRANS_RESP_CODE | 1-1 Mapping |
| Bronze | bz_shipment | TRLR_GEN_CODE | Source | SHIPMENT | TRLR_GEN_CODE | 1-1 Mapping |
| Bronze | bz_shipment | TRLR_SIZE | Source | SHIPMENT | TRLR_SIZE | 1-1 Mapping |
| Bronze | bz_shipment | TRLR_TYPE | Source | SHIPMENT | TRLR_TYPE | 1-1 Mapping |
| Bronze | bz_shipment | UN_NUMBER_ID | Source | SHIPMENT | UN_NUMBER_ID | 1-1 Mapping |
| Bronze | bz_shipment | UPDATE_SENT | Source | SHIPMENT | UPDATE_SENT | 1-1 Mapping |
| Bronze | bz_shipment | USE_BROKER_AS_CARRIER | Source | SHIPMENT | USE_BROKER_AS_CARRIER | 1-1 Mapping |
| Bronze | bz_shipment | VEHICLE_CHECK_START_DTTM | Source | SHIPMENT | VEHICLE_CHECK_START_DTTM | 1-1 Mapping |
| Bronze | bz_shipment | VOLUME_UOM_ID_BASE | Source | SHIPMENT | VOLUME_UOM_ID_BASE | 1-1 Mapping |
| Bronze | bz_shipment | WAVE_ID | Source | SHIPMENT | WAVE_ID | 1-1 Mapping |
| Bronze | bz_shipment | WAYPOINT_HANDLING_COST | Source | SHIPMENT | WAYPOINT_HANDLING_COST | 1-1 Mapping |
| Bronze | bz_shipment | WAYPOINT_TOTAL_COST | Source | SHIPMENT | WAYPOINT_TOTAL_COST | 1-1 Mapping |
| Bronze | bz_shipment | WEIGHT_UOM_ID_BASE | Source | SHIPMENT | WEIGHT_UOM_ID_BASE | 1-1 Mapping |
| Bronze | bz_shipment | WMS_STATUS_CODE | Source | SHIPMENT | WMS_STATUS_CODE | 1-1 Mapping |

## 3. Data Mapping – bz_shipment (metadata columns)

| Target Layer | Target Table | Target Field | Source Layer | Source Table | Source Field | Transformation Rule |
|---|---|---|---|---|---|---|
| Bronze | bz_shipment | load_timestamp | System | N/A | N/A | System generated: current_timestamp() |
| Bronze | bz_shipment | update_timestamp | System | N/A | N/A | System generated: current_timestamp() |
| Bronze | bz_shipment | source_system | System | N/A | N/A | Parameter: source system |

## 4. Data Mapping – bz_audit_log

The audit table is populated by the Bronze load process. It has no source table column. The rules below follow the audit design in the logical and physical models; the physical model does not define the value rules, so the rules are stated as inferences.

| Target Layer | Target Table | Target Field | Source Layer | Source Table | Source Field | Transformation Rule |
|---|---|---|---|---|---|---|
| Bronze | bz_audit_log | record_id | System | N/A | N/A | System generated: unique audit record identifier (inferred) |
| Bronze | bz_audit_log | source_table | System | N/A | N/A | Parameter: source table name (e.g. SHIPMENT) |
| Bronze | bz_audit_log | load_timestamp | System | N/A | N/A | System generated: current_timestamp() |
| Bronze | bz_audit_log | processed_by | System | N/A | N/A | Parameter: name of the load process / user (inferred) |
| Bronze | bz_audit_log | processing_time | System | N/A | N/A | System generated: elapsed load time in seconds (inferred) |
| Bronze | bz_audit_log | status | System | N/A | N/A | System generated: load status (inferred) |
| Bronze | bz_audit_log | row_count | System | N/A | N/A | System generated: number of rows loaded (inferred) |

## 5. Coverage Summary

| Target Table | Columns in physical model | Mapped rows | Breakdown |
|---|---|---|---|
| bz_shipment | 385 | 385 | 382 source columns (1-1 Mapping) + 3 metadata columns |
| bz_audit_log | 7 | 7 | 7 system generated / parameter columns |

Excluded from the mapping: source column SHIPMENT_ID (primary key), which is not carried into Bronze, as defined in the physical model.

## 6. Assumptions

- Source Layer is named "Source" because the source data model gives no schema name; the physical model notes that the `tms_landing` landing schema was not used.
- Data types follow the physical model (VARCHAR → STRING, DATETIME → TIMESTAMP). No values are changed, so these are type declarations only, not transformations.
- No deduplication, null handling, defaulting or validation rules are applied at Bronze. The NOT NULL flags in the source are not enforced.
- PII columns keep their raw values at Bronze; masking is left to governance, as stated in the physical model.

## 7. API Cost Calculation

apiCost: computed by the AAVA coordinator from token usage (see run report)
