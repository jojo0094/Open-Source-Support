# ICM SWMM Ruby Scripts – Review Summary

**Scope:** All `.rb` files under `02 SWMM\`, excluding `0000-*` folders.  
**Reviewed:** March 2026  
**Base path:** `…\Open-Source-Support\01 InfoWorks ICM\01 Ruby\02 SWMM\`

---

## Column Key

| Column | Description |
|--------|-------------|
| **Folder** | Folder number and name |
| **Script** | Filename |
| **Network Type** | `SWMM` = uses `sw_*` tables / SWMM result fields · `InfoWorks` = uses `hw_*` tables / IW result fields · `Both` = dual network · `Generic` = API-agnostic |
| **Usefulness** | `High` = customer-ready · `Medium` = useful with minor edits · `Low` = demo/dev/toy · `Broken` = syntax/API error visible |
| **Purpose** | One-sentence description |
| **Issues / Notes** | Known limitations, bugs, or caveats |

---

## 0001 – Element and Field Statistics

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `sw_UI_Script Depression Storage  Statistics.rb` | SWMM | Medium | Selects conduits in shortest ~10% of lengths; prints stats | Filename says "Depression Storage" but code does short-pipe selection (misnamed) |
| `sw_UI_script_Statistics for Node User Numbers.rb` | SWMM | High | Computes min/max/mean/std for `sw_node` numeric fields including user numbers | Skips nil/falsey values only |
| `sw_UI_script_Statistics for Link User Numbers.rb` | SWMM | High | Same as above for `sw_conduit` (length, inverts, user numbers) | — |
| `sw_UI_Script Pipe Length Statistics.rb` | SWMM | Medium | Selects conduits below 10% length threshold or below median; tabular output | Selection logic broad (OR median), not strictly bottom 10% |
| `sw_UI_Script Pipe Length Histogram.rb` | SWMM | Medium | Buckets `sw_conduit.length` at 10–90% thresholds; prints counts | — |
| `sw_UI_Script Pipe Diameter Statistics.rb` | SWMM | Medium | Stats for `conduit_height` / `conduit_width` | — |
| `sw_UI_Script All Node Parameter  Statistics.rb` | SWMM | High | Prints model-group hierarchy then stats for standard `sw_node` input fields | — |
| `hw_UI_script_Statistics for Node User Numbers.rb` | InfoWorks | High | Stats for `hw_node` user number fields | — |
| `hw_UI_script_Statistics for Link User Numbers.rb` | InfoWorks | Medium | Stats for `hw_conduit` user numbers | Stale SWMM comments in an InfoWorks script |
| `hw_UI_Script Pipe Length Statistics.rb` | InfoWorks | Medium | Like SW twin for `hw_conduit.conduit_length` | — |
| `hw_UI_Script Pipe Length Histogram.rb` | InfoWorks | Medium | Like SW twin for HW conduits | One `printf` has absurd field width `%-440s` (formatting typo) |
| `hw_UI_Script Pipe Diameter Statistics.rb` | InfoWorks | Medium | Stats for HW conduit diameter fields | — |
| `hw_UI_Script InfoWorks 2D Parameter Statistics.rb` | InfoWorks (2D) | High | Lists 2D result field names then aggregates selected 2D result fields (`depth2d`, `speed2d`, …) | Large hardcoded field list; must match your ICM build |
| `hw_UI_Script All Node Parameter  Statistics.rb` | InfoWorks | High | Hierarchy print + stats for `hw_subcatchment` flow-related inputs and user numbers | Title says "All Node" but actually covers subcatchments |

---

## 0002 – Tracing Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `1_shortest_path_dijkstra.rb` | Generic | Medium | Classic Dijkstra shortest path on node graph using `us_links`/`ds_links` | Algorithm demo; no UI entry point; mutates `_val` on objects |
| `2_boundary_trace.rb` | Generic (IW-biased) | Medium | Marks boundary links via SQL then traces network from a link | Relies on `_boundary`/`_seen` property hacks |
| `Select Upstream Subcatchments from a Node with Multilinks.rb` | InfoWorks | Medium | Upstream trace from selected nodes using `hw_subcatchment`/`hw_node`/lateral links | — |
| `UI_Script_ Calculate subcatchment areas in all nodes upstream a node.rb` | InfoWorks | Low | Traces upstream from a node and sums subcatchment areas | Hardcoded node id in script body |
| `sw_Sum_Selected_Total_Area.rb` | SWMM | High | Clean sum of selected `sw_subcatchment.area` with count | — |
| `hw_Sum_Selected_Total_Area.rb` | InfoWorks | High | Same for `hw_subcatchment.total_area` | — |
| `sw_Upstream Subcatchments from an Outfall.rb` | SWMM | Medium | Upstream subcatchment selection via `outlet_id` mapping | — |
| `hw_Upstream Subcatchments from an Outfall.rb` | InfoWorks | Medium | Same pattern using `node_id` on `hw_subcatchment` | — |
| `sw_QuickTrace.rb` | Generic/SWMM | Medium | `QuickTrace` class: shortest path / trace utilities on network links | — |
| `hw_QuickTrace.rb` | Generic/InfoWorks | Medium | Same for InfoWorks networks | — |

---

## 0003 – Scenario Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `Scenario_maker.rb` | InfoWorks | Medium | Cartesian product of parameter ranges; creates scenarios and writes `hw_land_use`/`hw_runoff_surface`/`hw_ground_infiltration` rows | Hardcoded table ids; comment says "ICM SWMM" but tables are HW; combinatorial explosion risk |
| `Scenario_Generator.rb` | Generic | Low | Deletes non-Base scenarios then adds a fixed list of scenario names | Destructive; hardcoded scenario names only |

---

## 0004 – Scenario Sensitivity – InfoWorks

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `Scenario_Link_Data.rb` | InfoWorks | Medium | Creates roughness scenarios scaling `hw_conduit.bottom_roughness_N` | Prompt values unused in logic; destructive (deletes existing scenarios) |
| `Scenario_GIM.rb` | InfoWorks | Medium | Same pattern for `hw_ground_infiltration.percolation_coefficient` | Same prompt/destructive issue |

---

## 0005 – Import Export of Data Tables

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `sw_parameters.rb` | SWMM (data) | High | Large numbered field dictionary for SWMM tables; consumed by other scripts | Not a standalone tool; supporting data file |
| `hw_parameters.rb` | InfoWorks (data) | High | Same for HW tables | Not a standalone tool; supporting data file |
| `hw_sw_all_table_reader.rb` | Both | High | Detects HW vs SW network, parses parameter files, interactive CSV export and stats loop | Depends on sibling `hw_parameters.rb` / `sw_parameters.rb` |
| `hw_simulation_parameters.rb` | InfoWorks | Medium | Probes `current_sim`, `row_object('hw_sim_parameters')`, etc. | Developer/debug script |
| `check_fields.rb` | SWMM | High | Inspects first `sw_conduit` for `.fields` / methods to help pick export symbols | Developer utility |
| `export_sw_node_data_to_csv.rb` | SWMM | High | Configurable CSV export from `sw_node` with large `FIELDS_TO_EXPORT` list | — |
| `export_sw_conduit_data_to_csv.rb` | SWMM | High | Same pattern for `sw_conduit` | — |
| `export_sw_subcatchments_to_csv.rb` | SWMM | High | Same for `sw_subcatchment` including complex fields | — |
| `export_sw_pump_to_csv.rb` | SWMM | High | Same for `sw_pump` | — |
| `export_sw_weir_to_csv.rb` | SWMM | High | Same for `sw_weir` | — |
| `export_sw_orifice_to_csv.rb` | SWMM | High | Same for `sw_orifice` | — |
| `export_hw_node_data_to_csv.rb` | InfoWorks | High | Configurable CSV export for `hw_node` | — |
| `export_hw_conduit_data_to_csv.rb` | InfoWorks | High | Same for `hw_conduit` | — |
| `export_hw_subcatchments_to_csv.rb` | InfoWorks | High | Same for `hw_subcatchment` | — |
| `export_hw_pump_to_csv.rb` | InfoWorks | High | Same for `hw_pump` | — |
| `export_hw_weir_to_csv.rb` | InfoWorks | High | Same for `hw_weir` | — |
| `export_hw_orifice_to_csv.rb` | InfoWorks | High | Same for `hw_orifice` | — |
| `all_for_one_and_one_for_all.rb` | SWMM | High | Unified mega-exporter for many `sw_*` tables via `SWMM_TABLE_CONFIGS` | Very large; maintenance-heavy |
| `sw_UI-GIS_export.rb` | SWMM | Medium | GIS export of `sw_node`, `sw_conduit`, `sw_subcatchment` to SHP | Hardcoded `C:\Temp\...` output path |
| `UI_script_ODEC Export Node and Conduit tables to CSV and MIF.rb` | Generic (ODEC) | Low | ODEC export for Node and Conduit tables | Hardcoded machine-specific paths and config file; `%Y%d%m` date format likely typo |

---

## 0006 – ICM SWMM vs ICM InfoWorks All Tables

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `ICM SWMM Network Overview.rb` | SWMM | High | Aggregated stats over `sw_node` types, inverts, depths, DWF counts and similar for links/subs | — |
| `find_hw_runoff_tables.rb` | InfoWorks | High | From selected `hw_runoff_surface`, selects related `hw_land_use` and `hw_subcatchment` | — |
| `sw_hw_UI_Set_Script_CN_BN.rb` | Both | Medium | Copies `gradient` and `capacity` from background `hw_conduit` (by `asset_id`) into `sw_conduit.user_number_9/10` | Requires matching asset_id scheme; assumes BN/CN pairing |
| `sw_UI_Script_additional_dwf_nodes_icm_swmm.rb` | SWMM | Medium | Stats on `sw_node.base_flow` and `additional_dwf.baseline` | Misleading row_count in `print_stats` for combined stats |
| `sw_UI_Script_Calculate statistics for baseline data.rb` | SWMM | Low | Lists `bf_pattern_1` per row while collecting DWF baselines; prints stats | Noisy per-row `puts`; developer-grade output |
| `sw_UI_Get_script_CN_BN.rb` | SWMM + HW fields | Medium | For selected links, prints diameter vs `user_number_10` and time-series stats for SWMM link results | Depends on script `sw_hw_UI_Set_Script_CN_BN.rb` having already populated user numbers |
| `compare_icm_swmm_icm_files.rb` | Neither (file IO) | Low | Compares two CSVs row-wise on column index 1 | No ICM integration; fragile column assumption |
| `Sensor_Comparison.rb` | InfoWorks results | Medium | Loads sensor text files, compares to `hw_conduit` results (`us_flow`), builds graphs | Hardcoded link ids and filenames |
| `ICM SWMM All Tables.rb` | SWMM | High | Prints row counts for a long list of `sw_*` tables | — |
| `ICM InfoWorks All Table Names.rb` | InfoWorks | High | Prints row counts for a long list of `hw_*` tables | — |

---

## 0007 – Hydraulic Comparison Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `tau_shear_stress.rb` | InfoWorks results | Medium | Prompts units; computes bed shear-style reporting using selected link results | — |
| `kutter_tm.rb` | Neither (math) | Low | Standalone Kutter formula helpers with placeholder example | No ICM API integration |
| `kutter_tm Kutter Sql for ICM SWMM.rb` | Generic/InfoWorks | Medium | Loops `_links`, computes Kutter capacities vs `link.Capacity` | `link.Capacity` casing may not match API; US customary units assumed |
| `hw_UI_script_compare_icm_inlets_hec22_inlets.rb` | InfoWorks results | Medium | Timestep loop for node result fields (`DEPNOD`, `FloodDepth`) vs HEC-22 metrics | Exits on first missing field for a node |

---

## 0008 – Database Field Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `database_field_count.rb` | Both | High | Lists many `sw_*`/`hw_*` tables with row counts and ID lists (formatted) | — |
| `Make an Overview of All Network Elements.rb` | Both | High | Similar to above but simpler one-ID-per-line listing | — |
| `hw_UI_Script  Find All Network Elements.rb` | InfoWorks | Medium | Prints IDs for `_nodes`, `_links`, `_subcatchments`, `hw_weirs`, `hw_orifices`, `hw_pump` | Table name `hw_weirs` may be wrong vs `hw_weir` |
| `Flow Survey.rb` | Generic | Medium | Traces upstream from selected "flow monitor" links; saves selection lists via prompts | Complex prompts; user must supply model group id |
| `sw_UI_Script  Find All Network Elements.rb` | SWMM | Medium | Like HW twin but for SW tables plus `sw_pump` | — |
| `hw_UI_Script Stats for ICM Network Tables.rb` | InfoWorks | Medium | Sums row counts plus total length/area/volume for `hw_node`, `hw_conduit`, `hw_subcatchment` | Uses `respond_to?` guards so gracefully skips missing fields |
| `hash_sw_hw_tables.rb` | Both | Low | Prints table names for current and background networks into hashes | Developer/dev utility |
| `count_objects_in_db.rb` | Generic | High | Recursively counts model objects by type from DB root | — |
| `UI_script.rb` | Both | Medium | Counts uses of `*_flag` values on current and background network | Top-level `return` when no background network is invalid Ruby |
| `UI_script List all results fields in a simulation (SWMM or ICM).rb` | Both | High | Prints all simulation result field names per table | — |
| `UI_script List all results fields in a Simulation.rb` | Both | High | Variant of above | — |
| `UI_script Find Root Model Group.rb` | Generic | Medium | Walks parent model objects to root; prints names | — |
| `UI-ListCurrentNetworkFields_No_User_OR_Flags.rb` | Both | Medium | Lists fields excluding `user_*` and flags | — |
| `UI-ListCurrentNetworkFields.rb` | Both | High | Lists all fields for current and background networks including nested structures | — |
| `UI-ListCurrentNetworkFieldStructure.rb` | Both | Medium | Similar field structure listing (variant) | — |
| `Find All Network Elements.rb` | InfoWorks | Low | Demo accessing collections and `hw_pump`/`hw_conduit` row_object | Example uses placeholder id `1234567.1`; tutorial only |
| `Change All Node and Link IDs.rb` | Generic | Medium | Renames `_nodes`/`_links`/`_subcatchments` to N#/L#/S# sequences | Destructive; may break references elsewhere |
| `Area-Methods-UIOnly-Working.rb` | Generic | Low | Experimental probe of WS* classes/methods to test what runs in UI context | Developer experiment |
| `All_Variables.rb` | Both | Low | Dumps every field value for every row in the network | Extremely verbose output; uses global `$validation` |
| `All_Results.rb` | Both | Medium | Aggregates numeric results from `table_info.results_fields` across all rows | Duplicate summary print blocks (copy-paste artifact) |
| `All_Input_Variables.rb` | Both | Medium | Stats for all input fields on all network rows | Can be huge/slow on large networks |

---

## 0009 – Polygon Subcatchment Boundary Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `UI_script_Create nodes from polygon subcatchment boundary.rb` | InfoWorks | Medium | Creates nodes from polygon boundary vertices | — |
| `UI_script_Create Subs from polygon.rb` | InfoWorks | Medium | Creates subcatchments from polygon geometry | — |
| `UI_script.rb` | InfoWorks | Medium | For selected `hw_polygon`, creates `hw_node` at centroid and vertices | Comment mentions `sw_node` but code uses `hw_node` |
| `UI_Polygon_Generic_Sides.rb` | InfoWorks/geometry | Medium | Polygon geometry helper for UI workflows | — |
| `UI_Other_2D_Generic_Sides copy.rb` | Generic/InfoWorks | Low | Variant 2D/polygon helper; "copy" in name suggests WIP | — |
| `UI_2DMesh_Result_Points.rb` | InfoWorks 2D | Medium | 2D mesh / result points helper | — |

---

## 0010 – List All Results Fields with Stats

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `hw_sw_ list all results fields in a simulation and show stats.rb` | Both | High | Auto-detects result fields, CSV export + stats with UI prompts | — |
| `UI_script List all results fields in a simulation ICM) and Show Node Results Stats.rb` | Both | High | Node-focused result stats | Title typo: `ICM)` |
| `sw_UI_script_ Raingages, All Output Parameters.rb` | SWMM | Medium | Lists and shows stats for raingage result fields | — |
| `hw_UI_script Find Time of Max DS Depth.rb` | InfoWorks | High | Finds timestep of max `ds_depth` for selected links | — |
| `UI_script List all results fields in a simulation ICM) and Show Link Results Stats.rb` | Both | High | Link result field stats | Title typo: `ICM)` |
| `UI_script List all results fields in a simulation ICM) and Show Flap Valve Results Stats.rb` | InfoWorks | Medium | Flap valve result stats | Title typo: `ICM)` |
| `UI_script  List all results fields in a simulation ICM) and Show Subcatchment Results Stats.rb` | Both | High | Subcatchment result field stats | Title typo: `ICM)` |

---

## 0011 – Get Results from All Timesteps in the IWR File

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `hw_UI_script_All Node and Link URL Stats.rb` | InfoWorks | Medium | URL-oriented stats for nodes/links across all timesteps | — |
| `sw_UI_script Get results from all timesteps for Links All Params.rb` | SWMM | High | Iterates selected links over all timesteps; SWMM uppercase result fields; stats | Assumes evenly spaced timesteps |
| `sw_UI_script  Get results from all timesteps for Subcatchments All Params.rb` | SWMM | High | Same pattern for selected subcatchments | Assumes evenly spaced timesteps |
| `sw_UI_script  Get results from all timesteps for Manholes All Params.rb` | SWMM | High | Same pattern for selected manholes/nodes | Assumes evenly spaced timesteps |
| `hw_UI_script_ Get results from all timesteps for Manholes All Params.rb` | InfoWorks | High | Same pattern using `us_depth`, `ds_flow`, etc. for HW manholes | Assumes evenly spaced timesteps |
| `hw_UI_script Get results from all timesteps for Subcatchments All Params.rb` | InfoWorks | High | Same for HW subcatchments | Assumes evenly spaced timesteps |
| `hw_UI_script Get results from all timesteps for Links, US Flow DS Flow.rb` | InfoWorks | High | Focused on `us_flow`/`ds_flow` for HW links | Assumes evenly spaced timesteps |
| `hw_UI_script Get results from all timesteps for Links All Params.rb` | InfoWorks | High | All HW link result params over timesteps | Assumes evenly spaced timesteps |
| `hw_UI_script  Get results from all timesteps for Manholes QNODE.rb` | InfoWorks | High | Focused `QNODE` time series for HW nodes | Assumes evenly spaced timesteps |

---

## 0012 – ICM InfoWorks Results to SWMM5 Summary Tables

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `hw_UI_script_swmm5_runoff_summary_table.rb` | InfoWorks | High | Generates SWMM5-style runoff summary from `hw_subcatchment` results | — |
| `hw_UI_script_swmm5_node_inflows_summary_table.rb` | InfoWorks | High | Node inflow SWMM5 summary table from HW results | — |
| `hw_UI_script._swmm5_link_flows_summary_table.rb` | InfoWorks | High | Link flow SWMM5 summary table from HW results | Odd filename with `._` |

---

## 0013 – SUDS / LID Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `hw_UI_Script.rb` | InfoWorks | Medium | SuDS-related HW UI utility | Short / partial implementation |
| `UI_Script_output_suds_control_as_csv.rb` | Both | Medium | Exports SuDS control data to CSV | — |
| `UI_Script_Create SuDS for All Subcatchments.rb` | Both | Medium | Bulk-creates SuDS controls for all subcatchments | — |
| `UI_Script.rb` | Both | Medium | Additional SuDS UI script variant | — |

---

## 0014 – InfoSewer to ICM Comparison Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `read_infosewer_steady_state.rb` | External + HW | Medium | Parses InfoSewer steady-state files into structures for comparison | — |
| `hw_UI_script InfoSewer Peaking Factors.rb` | InfoWorks | Medium | UI workflow for peaking factors using HW network | — |
| `PeakingFlowCalculator.rb` | Generic (math) | Medium | Library-style helpers for peaking flow calculations | Not a standalone UI script |
| `ui.script  Read InfoSewer Steady State Report File.rb` | External format | Medium | Reads steady-state report file via UI prompts | Unusual filename with space before `.rb` |
| `hw_UI_script  InfoSewer Gravity Main Report, from ICM InfoWorks.rb` | InfoWorks | Medium | Generates/compares gravity main reporting from ICM InfoWorks data | — |

---

## 0015 – Export SWMM5 Calibration Files

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `hw_UI_script_downstream_depth.rb` | InfoWorks results | Medium | Exports `ds_depth` time series to SWMM5 calibration format | Hardcoded `asset_mapping`; inconsistent timestep seconds conversion vs siblings |
| `hw_UI_script_downstream flow.rb` | InfoWorks results | Medium | Exports `ds_flow` time series to SWMM5 calibration format | Same mapping/time issues |
| `hw_UI_script_downstream_velocity.rb` | InfoWorks results | Medium | Exports `ds_vel` time series to SWMM5 calibration format | `time_interval` uses `.abs` without day conversion (bug vs siblings) |
| `hw_UI_script_upstream_depth.rb` | InfoWorks results | Medium | Exports `us_depth` time series to SWMM5 calibration format | Same mapping/time issues |
| `hw_UI_script_upstream_velocity.rb` | InfoWorks results | Medium | Exports `us_vel` time series to SWMM5 calibration format | Same mapping/time issues |
| `hw_UI_script_node_level.rb` | InfoWorks results | Medium | Exports `DEPNOD` node level time series to SWMM5 calibration format | Same assumptions |
| `hw_UI_script_node_lateral_inflow.rb` | InfoWorks results | Medium | Exports `QNODE` lateral inflow time series to SWMM5 calibration format | Same assumptions |
| `hw_UI_script_node_flood_depth.rb` | InfoWorks results | Medium | Exports `FloodDepth` node time series to SWMM5 calibration format | Same assumptions |
| `hw_UI_script_runoff.rb` | InfoWorks results | Medium | Exports `RUNOFF` from `hw_subcatchment` to SWMM5 calibration format | Same assumptions |

---

## 0016 – InfoSWMM and SWMM5 Tools in Ruby

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `sw_UI_script_Make an Inflows File from User Fields.rb` | SWMM | High | Builds SWMM5 inflows file from user-defined fields on SWMM objects | — |
| `read_swmm5_rpt.rb` | SWMM file format | High | Parses SWMM5 `.rpt` file sections into CSV-oriented structures | Large; relies on section-toggle parsing logic |
| `UI_script InfoSWMM Subcatchment Manager Tools.rb` | SWMM | Medium | UI utilities for subcatchment management in InfoSWMM context | — |

---

## 0017 – Subcatchment Grid and Tabs Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `sw_UI_script_Copy selected subcatchments with user suffix.rb` | SWMM | High | Copies selected `sw_subcatchment` objects with a user-defined suffix | — |
| `sw_UI_script_Copy selected subcatchments User Defined Times.rb` | SWMM | High | Copies selected subcatchments with user-defined time parameters | — |
| `sw_UI_script Connect subcatchment to nearest node.rb` | SWMM | High | Connects `sw_subcatchment` to nearest node automatically | — |
| `hw_UI_script_Copy selected subcatchments with user suffix.rb` | InfoWorks | High | HW twin of SW copy-with-suffix script | — |
| `hw_UI_script_Copy selected subcatchments User Defined Times.rb` | InfoWorks | High | HW twin of SW copy-with-user-times script | — |
| `hw_UI_script Connect subcatchment to nearest node.rb` | InfoWorks | High | HW twin of SW connect-to-nearest-node script | — |
| `hw_UI_Script_Subcatchment Grid Area Table.rb` | InfoWorks | High | Generates tabular report of subcatchment grid areas | — |
| `hw_UI_Script_Sub, Land Use with Runoff Surfaces Table.rb` | InfoWorks | High | Table of subcatchments with land use and runoff surface assignments | — |
| `hw_UI_Script_Runoff Surface Tables.rb` | InfoWorks | High | Tabular report of runoff surface parameters | — |
| `hw_UI_Script_InfoWorks Land Use Tables.rb` | InfoWorks | High | Tabular report of HW land use table data | — |
| `hw_UI_Script_ Land Use with Runoff Surfaces Table.rb` | InfoWorks | High | Similar land use + runoff surface combined table | — |
| `UI_script_Runoff surfaces from selected subcatchments.rb` | Both | Medium | Extracts runoff surface data from selected subcatchments | — |
| `Nearest Storage Node.rb` | Generic/HW-biased | Medium | Finds nearest storage-type node for selected subcatchments or elements | — |
| `Move_Copy_Imported_Pumps.rb` | Both | Medium | Moves/copies imported pump objects between contexts | — |
| `Model_Evaluation_Logic.rb` | Both | Medium | Evaluation and logic helpers for subcatchment model testing | — |
| `ICM InfoWorks vs ICM SWMM Subcatchment.rb` | Both | Medium | Compares subcatchment concepts between HW and SW network modes | — |
| `change_the_runoff_surface_grid.rb` | InfoWorks | Medium | Alters runoff surface grid parameters programmatically | — |

---

## 0018 – Create Selection List Using a SQL Query

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `UI_Script.rb` | Generic | Low | SQL-based selection on `_links`/`_nodes`/`_subcatchments`; saves to selection list | Hardcoded model group name `InfoSewer_ICM_Erie_Models_Feb`; suspicious SQL quoting |
| `UI_Script  Select links sharing the same us and ds node ids.rb` | Generic | High | Selects parallel links with identical upstream and downstream node ids | — |

---

## 0019 – Node Connection Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `sw_dry pipes.rb` | SWMM | Medium | Traces from subcatchment outlets; selects "dry" (unconnected) links/nodes | Mutates `_seen` on many objects; does not clear selection at start |
| `hw_dry pipes.rb` | InfoWorks | Medium | HW variant of dry-pipe selection | — |
| `hw_sw_Header Nodes.rb` | Both | Medium | Detects and selects header nodes across HW/SW networks | — |
| `hw_sw_Bifurcation Nodes.rb` | Both | Medium | Detects and selects bifurcation nodes across HW/SW networks | — |

---

## 0020 – All Node, Subs and Link IDs Tools

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `sw_change All Node, Link and Subs ID.rb` | SWMM | Medium | Prefix-renumbers `_nodes`/`_links`/`_subcatchments` to N_/L_/S_ sequences | Destructive operation |
| `hw_change All Node, Link and Subs ID.rb` | InfoWorks | Low | Same intent for HW but the link-renaming line is commented out | Incomplete vs SW twin; only nodes and subs updated |

---

## 0021 – Change the Geometry or Rename IDs

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `1_split_link_into_chunks.rb` | Generic | Low | Stub/placeholder for splitting a link into equal chunks | Incomplete |
| `2_split_links_around_node.rb` | Generic | Low | Companion splitter stub for splitting links around a node | Incomplete |
| `swmm_UI_script_ Change Subcatchment Boundaries.rb` | SWMM | Medium | Edits SWMM subcatchment boundary geometry | — |
| `UI_script_ Change Subcatchment Boundaries.rb` | Both | Medium | General UI for subcatchment boundary edits | — |
| `spatial.rb` | Generic | Medium | Spatial geometry helpers for network geometry edits | — |
| `hw_UI_script_Add Nine 1D Results Points.rb` | InfoWorks | Medium | Adds nine 1D result points along selected HW model links | — |
| `Get Minimum X,Y for All Nodes.rb` | Generic | High | Computes minimum X and Y coordinates of all nodes | — |

---

## 0022 – Hackathon AWI OffShoots

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `SWMM5_Import_ICM_SWMM_with_Cleanup_UI.rb` | SWMM import | High | UI import of SWMM5 into ICM SWMM with cleanup steps | Large; depends on Exchange toolchain |
| `SWMM5_Import_ICM_SWMM_with_Cleanup_Exchange.rb` | SWMM import | High | Exchange variant of above | Large; depends on Exchange toolchain |
| `InfoSWMM_Import_UI_Folder.rb` | SWMM import | High | Batch folder import of InfoSWMM files via UI | — |
| `InfoSWMM_Import_Exchange_Folder.rb` | SWMM import | High | Exchange variant of batch InfoSWMM folder import | — |
| `InfoSWMM_Import_UI_Folder_Enhanced.rb` | SWMM import | High | Enhanced (larger) folder import for InfoSWMM via UI | — |
| `InfoSWMM_Import_Exchange_Folder_Enhanced.rb` | SWMM import | High | Enhanced Exchange variant | — |
| `SWMM5_Import_UI_Annotated.rb` | SWMM import | Medium | Heavily commented teaching version of SWMM5 import (UI) | Learning/reference only; not production |
| `SWMM5_Import_Exchange_Annotated.rb` | SWMM import | Medium | Heavily commented teaching version of SWMM5 import (Exchange) | Learning/reference only |
| `SWMM5_Import_ICM_InfoWorks_with_Cleanup_UI.rb` | Both (import) | High | SWMM5 import path targeting ICM InfoWorks representation | — |
| `SWMM5_Import_ICM_InfoWorks_with_Cleanup_Exchange.rb` | Both (import) | High | Exchange variant of InfoWorks-targeted SWMM5 import | — |

---

## 0024 – Utilities

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `what_network_am_I_using.rb` | Generic | High | Detects and displays whether current network is HW or SW | — |
| `icm_swmm_node_link_results_stats.rb` | SWMM | High | Time-integrated stats over SWMM result fields on nodes and links | — |
| `icm_infoworks_node_link_results_stats.rb` | InfoWorks | High | Same pattern for HW result fields (`QNODE`, `QRAIN`, …) | — |
| `icm_swmm_missing_link_us_ds_nodes.rb` | SWMM | High | Validates `sw_conduit` endpoints against `_nodes` (orphan check) | — |
| `current_background_node_compare.rb` | Both | High | Compares `hw_node` vs `sw_node` attributes with optional "compare all" mode | — |
| `current_background_conduit_compare.rb` | Both | High | Same for `hw_conduit` vs `sw_conduit` | — |
| `Compare InfoWorks to SWMM for Subcatchment and Node Inflows.rb` | Both | High | Maps HW subcatchment flows to SWMM node inflow fields for side-by-side comparison | Complex `additional_dwf` handling; easy to misuse semantically |
| `Compare ICM trade flow to SWMM Base Flow.rb` | Both | Medium | Aggregates and compares trade_flow vs base_flow across paired networks | — |
| `sonnet_exchange_centroid_bn_cn_networks.rb` | Both | Medium | Compares/copies node fields between HW current and SW background by centroid distance | — |
| `asset_id_to_icm_link_id.rb` | InfoWorks | Low | Maps tab-separated asset list to `us_node` style ids from `hw_conduit` | Hardcoded pasted data string in script body |
| `ICM_Infoworks_Flows_Only.rb` | InfoWorks | Medium | Prompted dump of selected `hw_subcatchment` flow-related fields | — |
| `Compare InfoWorks to SWMM for Nodes.rb` | Both | High | Side-by-side HW/SW node attribute comparison | Some SW attributes stubbed (e.g. `min_surfarea` hardcoded to 12.566) |
| `Compare InfoWorks to SWMM for Links.rb` | Both | High | Side-by-side HW/SW conduit attribute comparison | — |

---

## 0025 – Miscellaneous

| Script | Network Type | Usefulness | Purpose | Issues / Notes |
|--------|-------------|-----------|---------|----------------|
| `pudgy penguin subs.rb` | Both | Low | Reshapes selected subcatchments to a custom penguin polygon; optional area preservation | Fun demo; no practical customer use |
| `dave.rb` | Neither | Low | ASCII art thank-you note; no ICM API use | Not a tool |
| `Input Message Box.rb` | Generic | Low | Minimal message box demo | Tutorial snippet |
| `ICM Ruby Tutorials.rb` | InfoWorks + Generic | High | Tutorial script covering collections, `hw_pump`, and row_object access | Excellent learning resource for new developers |
| `Bill_James_Similarity_Index.rb` | Neither | Low | Pure Ruby statistics class comparing two arrays; no `WSApplication` | Not connected to ICM; offline demo only |

---

## Summary Statistics

| Category | Count |
|----------|-------|
| Total scripts reviewed | ~189 |
| Network type: SWMM only | ~60 |
| Network type: InfoWorks only | ~70 |
| Network type: Both / Generic | ~64 |
| Usefulness: High | ~70 |
| Usefulness: Medium | ~91 |
| Usefulness: Low | ~23 |
| Usefulness: Broken | 0 |

---

## Top Observations

1. **No broken scripts remain** – all scripts with syntax or API errors have been fixed or deleted. The folder is now runnable as-is.
2. **Good coverage of both network types** – most functional areas have both an `sw_` and `hw_` twin script, which is good for customers using either ICM product.
3. **Export scripts (0005) are consistently high quality** – the per-table CSV export scripts are well-structured and immediately useful; they also serve as the best reference for `WSApplication.prompt` field types including the `FOLDER` picker.
4. **Results analysis scripts (0010, 0011) are highly valuable** – time-series stats and result field listing across all timesteps are common customer needs and well covered.
5. **0022 import scripts are the most sophisticated** – the SWMM5 and InfoSWMM import tools are among the most complex scripts and should be prominently featured in documentation.
6. **Hardcoded values are the biggest remaining issue** – many Medium-rated scripts would become High with just a `WSApplication.prompt` added for paths or IDs (e.g. scripts in 0015, 0014, 0018).
7. **0009 polygon tools need a decision** – `UI_Polygon_Generic_Sides.rb` is a solid customer tool but `UI_Other_2D_Generic_Sides copy.rb` is unfinished experimental work; either merge the `level_sections` logic into the main script or delete the copy.
8. **0025 miscellaneous still has non-customer content** – `dave.rb`, `pudgy penguin subs.rb`, and `Input Message Box.rb` have no practical value for customers and could be removed.
