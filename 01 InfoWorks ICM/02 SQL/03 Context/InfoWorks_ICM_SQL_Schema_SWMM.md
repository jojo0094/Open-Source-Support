# InfoWorks ICM SQL Schema — SWMM Networks

**Last Updated:** March 24, 2026

**Load Priority:** LOOKUP — Load for any SWMM network SQL query requiring field names
**Load Condition:** CONDITIONAL — When user asks about SWMM fields, schemas, or object inventory

**Related Files:**
- `InfoWorks_ICM_SQL_Schema_Common.md` — Common fields (`user_text_*`, `user_number_*`), results schema (`sim.*`, `tsr.*`), relationship paths, Autodesk Help workflow
- `InfoWorks_ICM_SQL_Schema_InfoWorks.md` — InfoWorks network field names (separate file; do NOT mix with SWMM fields)
- `InfoWorks_ICM_SQL_Lessons_Learned.md` — Read FIRST — Critical field name gotchas

## Purpose

This file is the **authoritative field-name and object-manifest reference for SWMM networks**.

Use it when:
- A user is working with a SWMM network (indicated by `(SWMM)` in the context or query)
- The query involves field names from `sw_*` tables
- An object manifest or coverage check is needed for SWMM objects

**InfoWorks fields are in `InfoWorks_ICM_SQL_Schema_InfoWorks.md`. Do not mix field sets.**

Key distinction: SWMM uses `length` for conduit length while InfoWorks uses `conduit_length`. Conduit dimensions (`conduit_width`/`conduit_height`) are the same in both networks.

## Retrieval Rules for LLMs

1. Confirm the network type is **SWMM** before using this file.
2. Match the **object type** (Node, Conduit, Subcatchment, etc.).
3. Use exact strings from the `Database Field` column.
4. If a user gives a UI label, check the `UI Label` column.
5. If a field is not listed here, check `InfoWorks_ICM_SQL_Schema_Common.md` for common/results fields.
6. Do not invent field names.

## Critical SQL Reminders

InfoWorks ICM SQL is **not standard ANSI SQL**. Even if `Lessons_Learned.md` was not loaded, these rules are mandatory:

- **No CASE WHEN** — use `IIF(condition, true_val, false_val)` or `IF condition; ... ELSEIF ...; ELSE; ... ENDIF;`
- **No JOINs** — use dot-notation navigation (e.g., `us_node.ground_level`, `ds_links.width`)
- **Semicolons required** after every statement, including control flow (`IF;`, `ELSE;`, `ENDIF;`, `WEND;`)
- **LIKE uses `?` and `*`** — not `%` and `_` (e.g., `LIKE 'MH*'` not `LIKE 'MH%'`)

For full anti-pattern coverage, load `InfoWorks_ICM_SQL_Lessons_Learned.md`.

---

## SWMM Network Object Manifest

Source: Autodesk Help `Network Data Fields` index page.

### Nodes Grid

| Object | Data Fields Topic | Internal Table |
|--------|-------------------|----------------|
| Node | Node Data Fields (SWMM) | `sw_node` |
| Unit hydrograph group | Unit Hydrograph Group Data Fields (SWMM) | `sw_uh_group` |
| Unit hydrograph | Unit Hydrograph Data Fields (SWMM) | `sw_uh` |
| Storage curve | Storage Curve Data Fields (SWMM) | `sw_curve_storage` |
| Tidal curve | Tidal Curve Data Fields (SWMM) | `sw_curve_tidal` |

### Links Grid

| Object | Data Fields Topic | Internal Table |
|--------|-------------------|----------------|
| Conduit | Conduit Data Fields (SWMM) | `sw_conduit` |
| Shape curve | Shape Curve Data Fields (SWMM) | `sw_curve_shape` |
| Orifice | Orifice Data Fields (SWMM) | `sw_orifice` |
| Pump | Pump Data Fields (SWMM) | `sw_pump` |
| Pump curve | Pump Curve Data Fields (SWMM) | `sw_curve_pump` |
| Weir | Weir Data Fields (SWMM) | `sw_weir` |
| Weir curve | Weir Curve Data Fields (SWMM) | `sw_curve_weir` |
| Outlet | Outlet Data Fields (SWMM) | `sw_outlet` |
| Rating curve | Rating Curve Data Fields (SWMM) | `sw_curve_rating` |
| Transect | Transect Data Fields (SWMM) | `sw_transect` |
| Control curve | Control Curve Data Fields (SWMM) | `sw_curve_control` |

### Subcatchments Grid

| Object | Data Fields Topic | Internal Table |
|--------|-------------------|----------------|
| Subcatchment | Subcatchment Data Fields (SWMM) | `sw_subcatchment` |
| Land use | Land Use Data Fields (SWMM) | `sw_land_use` |
| Pollutant | Pollutant Data Fields (SWMM) | `sw_pollutant` |
| Snow pack | Snow Pack Data Fields (SWMM) | `sw_snow_pack` |
| LID control | LID Controls Data Fields (SWMM) | `sw_suds_control` |
| Underdrain curve | Underdrain Curve Data Fields (SWMM) | `sw_curve_underdrain` |
| Aquifer | Aquifer Data Fields (SWMM) | `sw_aquifer` |
| Soil | Soil Data Fields (SWMM) | `sw_soil` |

### Polygons Grid

| Object | Data Fields Topic | Internal Table |
|--------|-------------------|----------------|
| 2D zone | 2D Zone Data Fields (SWMM) | `sw_2d_zone` |
| Mesh zone | Mesh Zone Data Fields (SWMM) | `sw_mesh_zone` |
| Mesh level zone | Mesh Level Zone Data Fields (SWMM) | `sw_mesh_level_zone` |
| Porous polygon | Porous Polygon Data Fields (SWMM) | `sw_porous_polygon` |
| Roughness zone | Roughness Zone Data Fields (SWMM) | `sw_roughness_zone` |
| Roughness definition | Roughness Definition Data Fields (SWMM) | `sw_roughness_definition` |
| Polygon | Polygon Data Fields (SWMM) | `sw_polygon` |
| TVD connector | TVD Connector Data Fields (SWMM) | `sw_tvd_connector` |
| Spatial rain zone | Spatial Rain Zone Data Fields (SWMM) | `sw_spatial_rain_zone` |
| Spatial rain source | Spatial Rain Source Data Fields (SWMM) | `sw_spatial_rain_source` |

### Lines Grid

| Object | Data Fields Topic | Internal Table |
|--------|-------------------|----------------|
| General line | General Line Data Fields (SWMM) | `sw_general_line` |
| Porous wall | Porous Wall Data Fields (SWMM) | `sw_porous_wall` |
| 2D boundary | 2D Boundary Line Data Fields (SWMM) | `sw_2d_boundary_line` |
| Head unit flow | Head Unit Flow Data Fields (SWMM) | `sw_head_unit_discharge` |

### Points Grid

| Object | Data Fields Topic | Internal Table |
|--------|-------------------|----------------|
| Rain gage | Rain Gage Data Fields (SWMM) | `sw_raingage` |

---

## SWMM Field Tables

All SWMM field tables are indexed here. For common fields (`user_text_*`, `user_number_*`, `hyperlinks`, `notes`) and results fields (`sim.*`, `tsr.*`) see `InfoWorks_ICM_SQL_Schema_Common.md`.

### Nodes

#### Node (`sw_node`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| Node ID | `node_id` | scalar | |
| Node Type | `node_type` | scalar | 'JUNCTION', 'OUTFALL', 'DIVIDER', 'STORAGE' |
| X Coordinate | `x` | scalar | |
| Y Coordinate | `y` | scalar | |
| Route Subcatchment | `route_subcatchment` | scalar | |
| Unit Hydrograph ID | `unit_hydrograph_id` | scalar | linked UH group |
| Unit Hydrograph Area | `unit_hydrograph_area` | scalar | |
| Ground Level | `ground_level` | scalar | |
| Invert Elevation | `invert_elevation` | scalar | SWMM node invert |
| Maximum Depth | `maximum_depth` | scalar | SWMM node depth |
| Surcharge Depth | `surcharge_depth` | scalar | SWMM surcharge depth |
| Initial Depth | `initial_depth` | scalar | SWMM initial condition |
| Ponded Area | `ponded_area` | scalar | surface flooding ponded area |
| Flood Type | `flood_type` | scalar | |
| Flooding Discharge Coefficient | `flooding_discharge_coeff` | scalar | |
| Evaporation Factor | `evaporation_factor` | scalar | |
| Initial Moisture Deficit | `initial_moisture_deficit` | scalar | Green-Ampt infiltration |
| Suction Head | `suction_head` | scalar | Green-Ampt infiltration |
| Conductivity | `conductivity` | scalar | Green-Ampt saturated hydraulic conductivity |
| Outfall Type | `outfall_type` | scalar | 'FREE', 'NORMAL', 'FIXED', 'TIDAL', 'TIMESERIES' |
| Flap Gate | `flap_gate` | scalar | outfall flap gate |
| Tidal Curve ID | `tidal_curve_id` | scalar | outfall using sw_curve_tidal |
| Fixed Stage | `fixed_stage` | scalar | outfall fixed stage level |
| Storage Type | `storage_type` | scalar | 'TABULAR', 'FUNCTIONAL' |
| Storage Curve | `storage_curve` | scalar | links to sw_curve_storage |
| Functional Coefficient | `functional_coefficient` | scalar | A×H^B + C |
| Functional Constant | `functional_constant` | scalar |  |
| Functional Exponent | `functional_exponent` | scalar |  |
| Inflow Baseline | `inflow_baseline` | scalar | DWF baseline inflow |
| Inflow Scaling | `inflow_scaling` | scalar | DWF scaling factor |
| Inflow Pattern | `inflow_pattern` | scalar | DWF pattern ID |
| Base Flow | `base_flow` | scalar |  |
| Base Flow Pattern 1 | `bf_pattern_1` | scalar |  |
| Base Flow Pattern 2 | `bf_pattern_2` | scalar |  |
| Base Flow Pattern 3 | `bf_pattern_3` | scalar |  |
| Base Flow Pattern 4 | `bf_pattern_4` | scalar |  |
| Treatment | `treatment` | blob | Sub-fields: `treatment.pollutant`, `treatment.result`, `treatment.function` |
| Pollutant Inflows | `pollutant_inflows` | blob | Sub-fields: `pollutant_inflows.pollutant` |
| Additional DWF | `additional_dwf` | blob | Sub-fields: `.baseline`, `.bf_pattern_1`–`4` |
| Pollutant DWF | `pollutant_dwf` | blob | Sub-fields: `pollutant_dwf.pollutant` |

> Common data fields (`user_text_1`–`10`, `user_number_1`–`10`, `notes`, `hyperlinks`) apply to this object — see `Schema_Common.md`.

#### Unit Hydrograph Group (`sw_uh_group`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Group identifier |
| Rain Gage ID | `raingage_id` | scalar | Linked rain gage |
| Unit Hydrograph All | `uh_all` | scalar | Group reference for all months |
| Unit Hydrograph Jan | `uh_jan` | scalar | Monthly group reference |
| Unit Hydrograph Feb | `uh_feb` | scalar | Monthly group reference |
| Unit Hydrograph Mar | `uh_mar` | scalar | Monthly group reference |
| Unit Hydrograph Apr | `uh_apr` | scalar | Monthly group reference |
| Unit Hydrograph May | `uh_may` | scalar | Monthly group reference |
| Unit Hydrograph Jun | `uh_jun` | scalar | Monthly group reference |
| Unit Hydrograph Jul | `uh_jul` | scalar | Monthly group reference |
| Unit Hydrograph Aug | `uh_aug` | scalar | Monthly group reference |
| Unit Hydrograph Sep | `uh_sep` | scalar | Monthly group reference |
| Unit Hydrograph Oct | `uh_oct` | scalar | Monthly group reference |
| Unit Hydrograph Nov | `uh_nov` | scalar | Monthly group reference |
| Unit Hydrograph Dec | `uh_dec` | scalar | Monthly group reference |

#### Unit Hydrograph (`sw_uh`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| Group ID | `group_id` | scalar | Parent group identifier |
| Month | `month` | scalar | Month selector |
| R1 | `R1` | scalar | RTK parameter |
| T1 | `T1` | scalar | RTK parameter |
| K1 | `K1` | scalar | RTK parameter |
| R2 | `R2` | scalar | RTK parameter |
| T2 | `T2` | scalar | RTK parameter |
| K2 | `K2` | scalar | RTK parameter |
| R3 | `R3` | scalar | RTK parameter |
| T3 | `T3` | scalar | RTK parameter |
| K3 | `K3` | scalar | RTK parameter |
| Dmax1 | `Dmax1` | scalar | RTK parameter |
| Drec1 | `Drec1` | scalar | RTK parameter |
| D01 | `D01` | scalar | RTK parameter |
| Dmax2 | `Dmax2` | scalar | RTK parameter |
| Drec2 | `Drec2` | scalar | RTK parameter |
| D02 | `D02` | scalar | RTK parameter |
| Dmax3 | `Dmax3` | scalar | RTK parameter |
| Drec3 | `Drec3` | scalar | RTK parameter |
| D03 | `D03` | scalar | RTK parameter |

#### Storage Curve (`sw_curve_storage`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Storage curve identifier |
| Depth | `data.depth` | blob | Curve data field |
| Surface Area | `data.surface_area` | blob | Curve data field |

#### Tidal Curve (`sw_curve_tidal`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Tidal curve identifier |
| Hour | `data.hour` | blob | Curve data field |
| Elevation | `data.elevation` | blob | Curve data field |

### Links

#### Conduit (`sw_conduit`)

> **CORRECTION:** Previous schema incorrectly listed `geom1`/`geom2`/`barrels`/`xsec_type` as SWMM conduit fields. SWMM SQL scripts in this repository confirm `conduit_width`, `conduit_height`, `number_of_barrels`, and `shape` are the correct SQL field names.

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | SWMM conduit identifier; InfoWorks uses `link_suffix` |
| Upstream Node ID | `us_node_id` | scalar | |
| Downstream Node ID | `ds_node_id` | scalar | |
| Length | `length` | scalar | SWMM conduit length; InfoWorks uses `conduit_length` |
| Shape | `shape` | scalar | SWMM cross-section shape code |
| Width / Diameter | `conduit_width` | scalar | Confirmed in SWMM SQL scripts; NOT `geom1` |
| Height | `conduit_height` | scalar | Confirmed in SWMM SQL scripts; NOT `geom2` |
| Number of Barrels | `number_of_barrels` | scalar | NOT `barrels` |
| Upstream Invert | `us_invert` | scalar | |
| Downstream Invert | `ds_invert` | scalar | |
| Manning's N | `Mannings_N` | scalar | Note case: capital M and N; confirmed from SWMM SQL scripts |
| Bottom Manning's N | `bottom_mannings_N` | scalar | composite roughness |
| Roughness Depth Threshold | `roughness_depth_threshold` | scalar |  |
| Darcy-Weisbach Roughness | `roughness_DW` | scalar |  |
| Hazen-Williams Roughness | `roughness_HW` | scalar |  |
| US Headloss Coefficient | `us_headloss_coeff` | scalar | |
| DS Headloss Coefficient | `ds_headloss_coeff` | scalar | |
| Average Headloss Coefficient | `av_headloss_coeff` | scalar |  |
| Initial Flow | `initial_flow` | scalar | initial condition |
| Max Flow | `max_flow` | scalar | flow limit |
| Sediment Depth | `sediment_depth` | scalar |  |
| Seepage Rate | `seepage_rate` | scalar |  |
| Culvert Code | `culvert_code` | scalar |  |
| Flap Gate | `flap_gate` | scalar |  |
| Branch ID | `branch_id` | scalar |  |
| Transect | `transect` | scalar | for IRREGULAR shape |
| Top Radius | `top_radius` | scalar | for ARCH shapes |
| Left Slope | `left_slope` | scalar | for TRAPEZOIDAL shape |
| Right Slope | `right_slope` | scalar | for TRAPEZOIDAL shape |
| Triangle Height | `triangle_height` | scalar | for TRIANGULAR shape |
| Bottom Radius | `bottom_radius` | scalar |  |
| Shape Curve | `shape_curve` | scalar | custom shape curve ID |
| Shape Exponent | `shape_exponent` | scalar |  |
| Horizontal Ellipse Size | `horiz_ellipse_size_code` | scalar | EPA SWMM standard size code |
| Vertical Ellipse Size | `vert_ellipse_size_code` | scalar |  |
| Arch Material | `arch_material` | scalar | for ARCH shapes |

> Common data fields (`user_text_1`–`10`, `user_number_1`–`10`, `notes`, `hyperlinks`) apply to this object — see `Schema_Common.md`.

#### Pump (`sw_pump`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | SWMM object identifier |
| Upstream Node ID | `us_node_id` | scalar | Common link identifier |
| Downstream Node ID | `ds_node_id` | scalar | Common link identifier |
| Ideal Pump | `ideal` | scalar | Ideal pump flag |
| Pump Curve | `pump_curve` | scalar | Linked pump curve |
| Initial Status | `initial_status` | scalar | SWMM control/status field |
| Start Up Depth | `start_up_depth` | scalar | SWMM control depth |
| Shut Off Depth | `shut_off_depth` | scalar | SWMM control depth |
| Branch ID | `branch_id` | scalar | Branch/control field |

#### Orifice (`sw_orifice`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | SWMM object identifier |
| Upstream Node ID | `us_node_id` | scalar | Common link identifier |
| Downstream Node ID | `ds_node_id` | scalar | Common link identifier |
| Type | `link_type` | scalar | Orifice type |
| Shape | `shape` | scalar | Orifice shape field |
| Orifice Height | `orifice_height` | scalar | Physical geometry field |
| Orifice Width | `orifice_width` | scalar | Physical geometry field |
| Invert | `invert` | scalar | Hydraulic geometry field |
| Discharge Coefficient | `discharge_coeff` | scalar | Hydraulic parameter |
| Flap Gate | `flap_gate` | scalar | Control field |
| Time To Open | `time_to_open` | scalar | Control field |
| Branch ID | `branch_id` | scalar | Branch/control field |

#### Outlet (`sw_outlet`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | SWMM object identifier |
| Upstream Node ID | `us_node_id` | scalar | Common link identifier |
| Downstream Node ID | `ds_node_id` | scalar | Common link identifier |
| Start Level | `start_level` | scalar | Outlet level field |
| Flap Gate | `flap_gate` | scalar | Control field |
| Rating Curve Type | `rating_curve_type` | scalar | Curve selector |
| Head Discharge ID | `head_discharge_id` | scalar | Linked curve table |
| Discharge Coefficient | `discharge_coefficient` | scalar | Hydraulic parameter |
| Discharge Exponent | `discharge_exponent` | scalar | Hydraulic parameter |
| Branch ID | `branch_id` | scalar | Branch/control field |

#### Weir (`sw_weir`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | SWMM object identifier |
| Upstream Node ID | `us_node_id` | scalar | Common link identifier |
| Downstream Node ID | `ds_node_id` | scalar | Common link identifier |
| Weir Type | `link_type` | scalar | Weir type |
| Crest | `crest` | scalar | Weir crest level |
| Weir Height | `weir_height` | scalar | SWMM weir geometry |
| Weir Width | `weir_width` | scalar | SWMM weir geometry |
| Left Slope | `left_slope` | scalar | SWMM weir geometry |
| Right Slope | `right_slope` | scalar | SWMM weir geometry |
| Variable Discharge Coefficient | `var_dis_coeff` | scalar | SWMM weir option |
| Discharge Coefficient | `discharge_coeff` | scalar | Hydraulic parameter |
| Sideflow Discharge Coefficient | `sideflow_discharge_coeff` | scalar | Hydraulic parameter |
| Weir Curve | `weir_curve` | scalar | Link to `sw_curve_weir` |
| Flap Gate | `flap_gate` | scalar | Control field |
| End Contractions | `end_contractions` | scalar | SWMM weir option |
| Secondary Discharge Coefficient | `secondary_discharge_coeff` | scalar | Hydraulic parameter |
| Allows Surcharge | `allows_surcharge` | scalar | SWMM behavior option |
| Width | `width` | scalar | Additional width field |
| Surface | `surface` | scalar | SWMM surface selector |
| Branch ID | `branch_id` | scalar | Branch/control field |

#### Weir Curve (`sw_curve_weir`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Weir curve identifier |
| Data Head | `data.head` | blob | Curve row field |
| Data Coefficient | `data.coefficient` | blob | Curve row field |
| Sideflow Head | `sideflow_data.head` | blob | Sideflow curve row field |
| Sideflow Coefficient | `sideflow_data.coefficient` | blob | Sideflow curve row field |
| Type | `type` | scalar | Curve type selector |

#### Transect (`sw_transect`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Transect identifier |
| Left Roughness | `left_roughness` | scalar | Transect roughness field |
| Right Roughness | `right_roughness` | scalar | Transect roughness field |
| Channel Roughness | `channel_roughness` | scalar | Transect roughness field |
| Left Offset | `left_offset` | scalar | Transect geometry field |
| Right Offset | `right_offset` | scalar | Transect geometry field |
| Width Factor | `width_factor` | scalar | Transect geometry field |
| Elevation Adjust | `elevation_adjust` | scalar | Transect adjustment field |
| Meander Factor | `meander_factor` | scalar | Transect adjustment field |
| Profile X | `profile.x` | blob | Transect profile blob field |
| Profile Z | `profile.z` | blob | Transect profile blob field |

#### Control Curve (`sw_curve_control`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Control curve identifier |
| Variable | `data.variable` | blob | Curve data field |
| Setting | `data.setting` | blob | Curve data field |

#### Pump Curve (`sw_curve_pump`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Pump curve identifier |
| Pump 1 Volume Increment | `pump1_data.volume_increment` | blob | Pump-curve data field |
| Pump 1 Outflow | `pump1_data.outflow` | blob | Pump-curve data field |
| Pump 2 Depth Increment | `pump2_data.depth_increment` | blob | Pump-curve data field |
| Pump 2 Outflow | `pump2_data.outflow` | blob | Pump-curve data field |
| Pump 3 Head Difference | `pump3_data.head_difference` | blob | Pump-curve data field |
| Pump 3 Outflow | `pump3_data.outflow` | blob | Pump-curve data field |
| Pump 4 Continuous Depth | `pump4_data.continuous_depth` | blob | Pump-curve data field |
| Pump 4 Outflow | `pump4_data.outflow` | blob | Pump-curve data field |
| Type | `type` | scalar | Pump-curve type selector |

#### Rating Curve (`sw_curve_rating`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Rating curve identifier |
| Head | `data.head` | blob | Curve data field |
| Outflow | `data.outflow` | blob | Curve data field |

#### Shape Curve (`sw_curve_shape`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Shape curve identifier |
| Normalized Depth | `data.normalized_depth` | blob | Curve data field |
| Normalized Width | `data.normalized_width` | blob | Curve data field |

### Subcatchments

#### Subcatchment (`sw_subcatchment`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| Subcatchment ID | `subcatchment_id` | scalar | |
| Outlet ID | `outlet_id` | scalar | Primary receiving node/outlet |
| Drains To | `sw_drains_to` | scalar | Routing selector |
| Rain Gage ID | `raingauge_id` | scalar | Linked rain gage |
| Area | `area` | scalar | SWMM area; InfoWorks uses `contributing_area` |
| Width | `width` | scalar | Characteristic width |
| Hydraulic Length | `hydraulic_length` | scalar |  |
| Catchment Slope | `catchment_slope` | scalar |  |
| Percent Impervious | `percent_impervious` | scalar | **CORRECTED** from `percent_imperv`; confirmed in SWMM SQL scripts |
| Roughness Impervious | `roughness_impervious` | scalar | Manning's N for impervious surface |
| Roughness Pervious | `roughness_pervious` | scalar | Manning's N for pervious surface |
| Storage Impervious | `storage_impervious` | scalar | Depression storage impervious |
| Storage Pervious | `storage_pervious` | scalar | Depression storage pervious |
| Percent No Storage | `percent_no_storage` | scalar | Percentage with no depression storage |
| Route To | `route_to` | scalar | Routed-flow destination selector |
| Percent Routed | `percent_routed` | scalar | Percentage of flow routed |
| Infiltration | `infiltration` | scalar | Infiltration model type selector |
| Initial Infiltration | `initial_infiltration` | scalar | Horton/GA initial rate |
| Limiting Infiltration | `limiting_infiltration` | scalar | Horton/GA limiting rate |
| Decay Factor | `decay_factor` | scalar | Horton decay constant |
| Initial Abstraction Factor | `initial_abstraction_factor` | scalar |  |
| Drying Time | `drying_time` | scalar | Horton drying time |
| Max Infiltration Volume | `max_infiltration_volume` | scalar | Volume limit for Horton method |
| Initial Moisture Deficit | `initial_moisture_deficit` | scalar | Green-Ampt initial moisture deficit |
| Average Capillary Suction | `average_capillary_suction` | scalar | Green-Ampt suction head |
| Saturated Hydraulic Conductivity | `saturated_hydraulic_conductivity` | scalar | Green-Ampt saturated K |
| Curve Number | `curve_number` | scalar | CN method curve number |
| Initial Abstraction | `initial_abstraction` | scalar |  |
| Initial Abstraction Type | `initial_abstraction_type` | scalar |  |
| Runoff Model Type | `runoff_model_type` | scalar | Runoff model selector |
| Shape Factor | `shape_factor` | scalar |  |
| Time of Concentration | `time_of_concentration` | scalar |  |
| Snow Pack ID | `snow_pack_id` | scalar | Linked snow pack |
| Curb Length | `curb_length` | scalar | Gutter/curb length for inlet capture |
| Aquifer ID | `aquifer_id` | scalar | Linked aquifer |
| Aquifer Node ID | `aquifer_node_id` | scalar | Groundwater discharge node |
| Aquifer Elevation | `aquifer_elevation` | scalar | Aquifer/GW reference elevation |
| Aquifer Initial Groundwater | `aquifer_initial_groundwater` | scalar | Initial GW level |
| Aquifer Initial Moisture | `aquifer_initial_moisture_content` | scalar | Initial unsaturated zone moisture |
| Elevation | `elevation` | scalar | surface elevation |
| Groundwater Coefficient | `groundwater_coefficient` | scalar | Lateral GW flow coefficient |
| Groundwater Exponent | `groundwater_exponent` | scalar | Lateral GW flow exponent |
| Groundwater Threshold | `groundwater_threshold` | scalar | GW flow threshold depth |
| Lateral GWF Equation | `lateral_gwf_equation` | scalar |  |
| Deep GWF Equation | `deep_gwf_equation` | scalar |  |
| Surface Coefficient | `surface_coefficient` | scalar | Surface GW flow coefficient |
| Surface Depth | `surface_depth` | scalar |  |
| Surface Exponent | `surface_exponent` | scalar |  |
| Surface GW Coefficient | `surface_groundwater_coefficient` | scalar |  |
| Area Average Rain | `area_average_rain` | scalar |  |
| X Coordinate | `x` | scalar |  |
| Y Coordinate | `y` | scalar |  (note: capital X in results scripts, lowercase y) |
| N Perv Pattern | `n_perv_pattern` | scalar | time-varying roughness |
| D-Store Pattern | `dstore_pattern` | scalar |  |
| Infiltration Pattern | `infil_pattern` | scalar |  |
| Coverages | `coverages` | blob | Sub-fields: `.land_use`, `.area` |
| Loadings | `loadings` | blob | Sub-fields: `.pollutant`, `.build_up` |
| Soil | `soil` | blob | Sub-fields: `.soil`, `.area` |

> Common data fields (`user_text_1`–`10`, `user_number_1`–`10`, `notes`, `hyperlinks`) apply to this object — see `Schema_Common.md`.

#### Subcatchment LID / SuDS Controls (`sw_suds_control`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| Unit Surface Width | `suds_controls.unit_surface_width` | blob | SWMM SuDS/LID nested field |
| Area | `suds_controls.area` | blob | SWMM SuDS/LID nested field |
| Area % of Subcatchment | `suds_controls.area_subcatchment_pct` | blob | SWMM SuDS/LID nested field |
| Control Type | `suds_controls.control_type` | blob | SWMM SuDS/LID nested field |
| Drain To Node | `suds_controls.drain_to_node` | blob | SWMM SuDS/LID nested field |
| Drain To Subcatchment | `suds_controls.drain_to_subcatchment` | blob | SWMM SuDS/LID nested field |
| SuDS Control ID | `suds_controls.id` | blob | SWMM SuDS/LID nested field |
| Impervious Area Treated % | `suds_controls.impervious_area_treated_pct` | blob | SWMM SuDS/LID nested field |
| Initial Saturation % | `suds_controls.initial_saturation_pct` | blob | SWMM SuDS/LID nested field |
| Number of Units | `suds_controls.num_units` | blob | SWMM SuDS/LID nested field |
| Outflow To | `suds_controls.outflow_to` | blob | SWMM SuDS/LID nested field |
| Pervious Area Treated % | `suds_controls.pervious_area_treated_pct` | blob | SWMM SuDS/LID nested field |
| SuDS Structure | `suds_controls.suds_structure` | blob | SWMM SuDS/LID nested field |
| Surface | `suds_controls.surface` | blob | SWMM SuDS/LID nested field |

#### Land Use (`sw_land_use`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | SWMM land use identifier |
| Build Up Pollutant | `build_up.pollutant` | blob | Build-up blob field |
| Build Up Type | `build_up.build_up_type` | blob | Build-up blob field |
| Build Up Max Build Up | `build_up.max_build_up` | blob | Build-up blob field |
| Build Up Power Rate Constant | `build_up.power_rate_constant` | blob | Build-up blob field |
| Build Up Power Time Exponent | `build_up.power_time_exponent` | blob | Build-up blob field |
| Build Up Exp Rate Constant | `build_up.exp_rate_constant` | blob | Build-up blob field |
| Build Up Saturation Constant | `build_up.saturation_constant` | blob | Build-up blob field |
| Build Up Unit | `build_up.unit` | blob | Build-up blob field |
| Sweep Interval | `sweep_interval` | scalar | Surface management field |
| Sweep Removal | `sweep_removal` | scalar | Surface management field |
| Last Sweep | `last_sweep` | scalar | Surface management field |

#### Pollutant (`sw_pollutant`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Pollutant identifier |
| Units | `units` | scalar | Pollutant units |
| Rainfall Concentration | `rainfall_conc` | scalar | Concentration field |
| Groundwater Concentration | `groundwater_conc` | scalar | Concentration field |
| RDII Concentration | `rdii_conc` | scalar | Concentration field |
| Dry Weather Flow Concentration | `dwf_conc` | scalar | Concentration field |
| Initial Concentration | `init_conc` | scalar | Initial condition |
| Decay Coefficient | `decay_coeff` | scalar | Decay field |
| Snow Build Up | `snow_build_up` | scalar | Snow field |
| Co-Pollutant | `co-pollutant` | scalar | Co-pollutant name field |
| Co-Fraction | `co-fraction` | scalar | Co-pollutant fraction field |

#### Snow Pack (`sw_snow_pack`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | SWMM snow pack identifier |
| Plow Minimum Melt | `plow_min_melt` | scalar | Melt parameter |
| Impervious Minimum Melt | `imp_min_melt` | scalar | Melt parameter |
| Pervious Minimum Melt | `perv_min_melt` | scalar | Melt parameter |
| Plow Maximum Melt | `plow_max_melt` | scalar | Melt parameter |
| Impervious Maximum Melt | `imp_max_melt` | scalar | Melt parameter |
| Pervious Maximum Melt | `perv_max_melt` | scalar | Melt parameter |
| Plow Base Temperature | `plow_base_temp` | scalar | Temperature field |
| Impervious Base Temperature | `imp_base_temp` | scalar | Temperature field |
| Pervious Base Temperature | `perv_base_temp` | scalar | Temperature field |
| Fraction Plowable | `fraction_plowable` | scalar | Routing field |
| To Impervious | `to_impervious` | scalar | Routing fraction |
| To Pervious | `to_pervious` | scalar | Routing fraction |
| To Immediate Melt | `to_immediate_melt` | scalar | Routing fraction |
| To Subcatchment | `to_subcatchment` | scalar | Routing fraction |
| Subcatchment ID | `subcatchment_id` | scalar | Linked subcatchment |
| Plow Free Water | `plow_free_water` | scalar | Water content parameter |
| Impervious Free Water | `imp_free_water` | scalar | Water content parameter |
| Pervious Free Water | `perv_free_water` | scalar | Water content parameter |
| Plow Snow Depth | `plow_snow_depth` | scalar | Snow depth field |
| Impervious Snow Depth | `imp_snow_depth` | scalar | Snow depth field |
| Pervious Snow Depth | `perv_snow_depth` | scalar | Snow depth field |
| Plow Depth | `plow_depth` | scalar | Snow depth/routing field |
| Out of Watershed | `out_of_watershed` | scalar | Routing field |

#### Underdrain Curve (`sw_curve_underdrain`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Underdrain curve identifier |
| Depth | `data.depth` | blob | Curve data field |
| Factor | `data.factor` | blob | Curve data field |

#### Aquifer (`sw_aquifer`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Aquifer identifier |
| Soil Porosity | `soil_porosity` | scalar | Soil property |
| Soil Wilting Point | `soil_wilting_point` | scalar | Soil property |
| Soil Field Capacity | `soil_field_capacity` | scalar | Soil property |
| Conductivity | `conductivity` | scalar | Aquifer property |
| Conductivity Slope | `conductivity_slope` | scalar | Aquifer property |
| Tension Slope | `tension_slope` | scalar | Aquifer property |
| Evapotranspiration Fraction | `evapotranspiration_fraction` | scalar | ET property |
| Evapotranspiration Depth | `evapotranspiration_depth` | scalar | ET property |
| Seepage Rate | `seepage_rate` | scalar | Aquifer property |
| Elevation | `elevation` | scalar | Elevation field |
| Initial Groundwater | `initial_groundwater` | scalar | Initial condition |
| Initial Moisture Content | `initial_moisture_content` | scalar | Initial condition |
| Time Pattern ID | `time_pattern_id` | scalar | Linked time pattern |

#### Soil (`sw_soil`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| ID | `id` | scalar | Soil identifier |
| Initial Infiltration | `initial_infiltration` | scalar | Infiltration field |
| Limiting Infiltration | `limiting_infiltration` | scalar | Infiltration field |
| Decay Factor | `decay_factor` | scalar | Infiltration decay |
| Drying Time | `drying_time` | scalar | Infiltration drying |
| Max Infiltration Volume | `max_infiltration_volume` | scalar | Infiltration volume limit |
| Initial Moisture Deficit | `initial_moisture_deficit` | scalar | Green-Ampt field |
| Curve Number | `curve_number` | scalar | CN infiltration field |
| Average Capillary Suction | `average_capillary_suction` | scalar | Green-Ampt field |
| Saturated Hydraulic Conductivity | `saturated_hydraulic_conductivity` | scalar | Green-Ampt field |

### Polygons and 2D Objects

#### Polygon, TVD, and Spatial Rain (`sw_polygon`, `sw_tvd_connector`, `sw_spatial_rain_zone`, `sw_spatial_rain_source`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| Polygon ID | `polygon_id` | scalar | Use with `sw_polygon` |
| Polygon Category ID | `category_id` | scalar | Category field |
| Polygon Area | `area` | scalar | Polygon area |
| Polygon Boundary | `boundary_array` | blob | Polygon geometry |
| TVD Connector ID | `id` | scalar | Use with `sw_tvd_connector` |
| Input A | `input_a` | scalar | Expression input |
| Input B | `input_b` | scalar | Expression input |
| Input C | `input_c` | scalar | Expression input |
| Output Expression | `output_expression` | scalar | Expression text |
| Connected Object ID | `connected_object_id` | scalar | Link target ID |
| Spatial Rain Source Type | `source_type` | scalar | Use with `sw_spatial_rain_source` |
| Spatial Rain Source Start Time | `start_time` | scalar | Temporal field |
| Spatial Rain Source End Time | `end_time` | scalar | Temporal field |

#### 2D Zone, Roughness, and Mesh Support (`sw_2d_zone`, `sw_roughness_zone`, `sw_roughness_definition`, `sw_mesh_zone`, `sw_mesh_level_zone`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| 2D Zone ID | `zone_id` | scalar | Use with `sw_2d_zone` |
| 2D Zone Max Triangle Area | `max_triangle_area` | scalar | Mesh control field |
| 2D Zone Min Mesh Element Area | `min_mesh_element_area` | scalar | Mesh control field |
| 2D Zone Roughness | `roughness` | scalar | 2D roughness value |
| 2D Zone Roughness Definition ID | `roughness_definition_id` | scalar | Links to `sw_roughness_definition` |
| Roughness Zone Exclude From 2D Mesh | `exclude_from_2d_mesh` | scalar | Use with `sw_roughness_zone` |
| Roughness Zone Priority | `priority` | scalar | Override priority |
| Roughness Definition ID | `definition_id` | scalar | Use with `sw_roughness_definition` |
| Roughness Definition Bands | `number_of_bands` | scalar | Band count |
| Mesh Zone Polygon ID | `polygon_id` | scalar | Use with `sw_mesh_zone` |
| Mesh Zone Min Mesh Element Area | `min_mesh_element_area` | scalar | Mesh control field |
| Mesh Level Type | `level_type` | scalar | Use with `sw_mesh_level_zone` |
| Mesh Upper Limit Level | `upper_limit_level` | scalar | Level cap |
| Mesh Lower Limit Level | `lower_limit_level` | scalar | Level floor |
| Mesh Level Section Elevation | `level_sections.elevation` | blob | Nested mesh-level section field |

#### Porous Polygon (`sw_porous_polygon`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| Porosity | `porosity` | scalar | Polygon porosity |
| Crest Level | `crest_level` | scalar | Structure elevation |
| No Rainfall | `no_rainfall` | scalar | Rainfall exclusion flag |

### Lines

#### General Line, Porous Wall, 2D Boundary, and Head Unit Flow (`sw_general_line`, `sw_porous_wall`, `sw_2d_boundary_line`, `sw_head_unit_discharge`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| General Line ID | `line_id` | scalar | Use with `sw_general_line` |
| General Line Category | `category` | scalar | Classification field |
| General Line Length | `length` | scalar | Line length |
| Porous Wall Wall Removal Trigger | `wall_removal_trigger` | scalar | Use with `sw_porous_wall` |
| Porous Wall Porosity | `porosity` | scalar | Porosity field |
| Porous Wall Depth Threshold | `depth_threshold` | scalar | Trigger threshold |
| 2D Boundary Line Type | `line_type` | scalar | Use with `sw_2d_boundary_line` |
| 2D Boundary Head Unit Discharge ID | `head_unit_discharge_id` | scalar | Linked flow table |
| Head Unit Discharge Description | `head_unit_discharge_description` | scalar | Use with `sw_head_unit_discharge` |
| Head Unit Discharge Head | `HUDP_table.head` | blob | Nested table field |
| Head Unit Discharge Value | `HUDP_table.unit_discharge` | blob | Nested table field |

### Points

#### Rain Gage (`sw_raingage`)

| UI Label | Database Field | Type | Notes |
|----------|----------------|------|-------|
| Rain Gage ID | `raingage_id` | scalar | Rain gage identifier |
| X Coordinate | `x` | scalar | Geometry field |
| Y Coordinate | `y` | scalar | Geometry field |
| SCF | `scf` | scalar | Scale factor field |

---

## Simulation Results

Result fields use `tsr.ATTRIBUTE` syntax. See `InfoWorks_ICM_SQL_Schema_Common.md` for `tsr.*` metadata fields.

### Summary Results (`sim.*`)

The `sim.*` prefix returns summary results at the current timestep or maximum. No aggregate function is needed.

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Maximum Depth | `sim.max_depth` | result | SWMM node/conduit result |
| Maximum Flow | `sim.max_flow` | result | SWMM link result |

**Do not assume a `sim.*` suffix from one network type will work in the other.** InfoWorks-specific `sim.*` fields are in `Schema_InfoWorks.md`.

### Node Results (`sw_node`, `sw_outfall`, `sw_storage_node`)

#### Hydraulic time-varying results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Water depth | `tsr.DEPTH` | result | Time-varying |
| Hydraulic head | `tsr.HEAD` | result | Time-varying |
| Stored volume | `tsr.VOLUME` | result | Time-varying |
| Lateral inflow | `tsr.LATERAL_INFLOW` | result | Time-varying |
| Total inflow | `tsr.TOTAL_INFLOW` | result | Time-varying |
| Surface flooding | `tsr.FLOODING` | result | Time-varying |
| Pressure | `tsr.PRESSURE` | result | Time-varying |
| Inlet elevation | `tsr.INVERT_ELEVATION` | result | Time-varying |
| Head class | `tsr.HEAD_CLASS` | result | Time-varying; 0=below invert, 1=below crown, 2=below max depth, 3=surcharged |

#### Hydraulic maxima results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Max water depth | `tsr.MAX_DEPTH` | result | Maxima |
| Max hydraulic head | `tsr.MAX_HEAD` | result | Maxima |
| Max total inflow | `tsr.MAX_TOTAL_INFLOW` | result | Maxima |

#### Hydraulic summary results (non time-varying)

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Total flood volume | `tsr.TOTAL_FLOOD_VOLUME` | result | Summary |
| Total flood time | `tsr.TOTAL_FLOOD_TIME` | result | Summary |
| Flow volume difference | `tsr.FLOW_VOLUME_DIFFERENCE` | result | Summary |
| Total inflow volume | `tsr.TOTAL_INFLOW_VOLUME` | result | Summary; outfall nodes only |

#### Water quality results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| \<pollutant name\> | `tsr.<pollutant name>` | result | Time-varying; attribute is the pollutant name |

### Link Results (`sw_conduit`, `sw_pump`, `sw_orifice`, `sw_weir`, `sw_outlet`)

#### Conduit hydraulic time-varying results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Flow | `tsr.FLOW` | result | Time-varying |
| Depth | `tsr.DEPTH` | result | Time-varying |
| Velocity | `tsr.VELOCITY` | result | Time-varying |
| HGL | `tsr.HGL` | result | Time-varying; average of upstream and downstream node heads |
| Flow volume | `tsr.FLOW_VOLUME` | result | Time-varying |
| Flow class | `tsr.FLOW_CLASS` | result | Time-varying |
| Capacity (depth/max depth) | `tsr.CAPACITY` | result | Time-varying |
| Surcharged | `tsr.SURCHARGED` | result | Time-varying |
| Entry loss | `tsr.ENTRY_LOSS` | result | Time-varying |
| Exit loss | `tsr.EXIT_LOSS` | result | Time-varying |

#### Pump hydraulic time-varying results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Flow | `tsr.FLOW` | result | Time-varying |
| Depth | `tsr.DEPTH` | result | Time-varying |
| Upstream head | `tsr.UPSTREAM_HEAD` | result | Time-varying |
| Downstream head | `tsr.DOWNSTREAM_HEAD` | result | Time-varying |
| Head gain | `tsr.HEAD_GAIN` | result | Time-varying |
| Speed ratio | `tsr.SPEED_RATIO` | result | Time-varying |
| Useful power | `tsr.USEFUL_POWER` | result | Time-varying |

#### Orifice/Weir/Outlet hydraulic time-varying results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Flow | `tsr.FLOW` | result | Time-varying |
| Depth | `tsr.DEPTH` | result | Time-varying |

#### Conduit hydraulic maxima results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Max flow | `tsr.MAX_FLOW` | result | Maxima |
| Max depth | `tsr.MAX_DEPTH` | result | Maxima |
| Max capacity | `tsr.MAX_CAPACITY` | result | Maxima |
| Max velocity | `tsr.MAX_VELOCITY` | result | Maxima |

#### Pump/orifice/weir/outlet hydraulic maxima results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Max flow | `tsr.MAX_FLOW` | result | Maxima |
| Max depth | `tsr.MAX_DEPTH` | result | Maxima |

#### Pump hydraulic summary results (non time-varying)

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Min flow | `tsr.MIN_FLOW` | result | Summary |
| Percentage utilised | `tsr.PERCENT_UTILISED` | result | Summary |
| Power usage | `tsr.POWER_USAGE` | result | Summary |

#### Water quality results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| \<pollutant name\> | `tsr.<pollutant name>` | result | Time-varying; attribute is the pollutant name |
| Max \<pollutant name\> | `tsr.<pollutant name>` | result | Maxima; attribute is the pollutant name |

### Subcatchment Results (`sw_subcatchment`)

#### Hydraulic time-varying results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Rainfall | `tsr.RAINFALL` | result | Time-varying |
| Snow depth | `tsr.SNOW_DEPTH` | result | Time-varying |
| Evaporation loss | `tsr.EVAPORATION_LOSS` | result | Time-varying |
| Infiltration loss | `tsr.INFILTRATION_LOSS` | result | Time-varying |
| Runoff | `tsr.RUNOFF` | result | Time-varying |
| Groundwater flow | `tsr.GROUNDWATER_FLOW` | result | Time-varying |
| Groundwater elevation | `tsr.GROUNDWATER_ELEVATION` | result | Time-varying |
| Impervious runoff | `tsr.IMPERV_RUNOFF` | result | Time-varying |
| Pervious runoff | `tsr.PERV_RUNOFF` | result | Time-varying |

#### Hydraulic summary results (non time-varying)

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Total precipitation | `tsr.TOTAL_PRECIPITATION` | result | Summary |
| Total runon | `tsr.TOTAL_RUNON` | result | Summary |
| Total evaporation | `tsr.TOTAL_EVAPORATION` | result | Summary |
| Total infiltration | `tsr.TOTAL_INFILTRATION` | result | Summary |
| Total runoff depth | `tsr.TOTAL_RUNOFF_DEPTH` | result | Summary |
| Total runoff volume | `tsr.TOTAL_RUNOFF_VOLUME` | result | Summary |
| Peak runoff | `tsr.PEAK_RUNOFF` | result | Summary |
| Peak runoff time | `tsr.PEAK_RUNOFF_TIME` | result | Summary |
| Runoff coefficient | `tsr.RUNOFF_COEFFICIENT` | result | Summary |

#### Water quality results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| \<pollutant name\> | `tsr.<pollutant name>` | result | Time-varying; attribute is the pollutant name |
| Max \<pollutant name\> | `tsr.<pollutant name>` | result | Maxima; attribute is the pollutant name |

### Rain Gauge Results (`sw_raingage`)

#### Time-varying results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Cumulative rainfall | `tsr.RAINDPTH` | result | Time-varying; cumulative rainfall depth |
| Rainfall | `tsr.RAINFALL` | result | Time-varying; rainfall intensity |

### 2D Zone Results (`sw_2d_zone`)

#### Time-varying results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Direction | `tsr.ANGLE2D` | result | Time-varying; flow direction in radians from due East |
| Mean depth | `tsr.AVEDEPTH2D` | result | Time-varying; wet-portion average depth; subgrid models only |
| Depth | `tsr.DEPTH2D` | result | Time-varying; highest depth for subgrid elements |
| Eddy viscosity | `tsr.EDDYVISCOSITY2D` | result | Time-varying |
| Elevation | `tsr.elevation2d` | result | Time-varying; null when element is dry |
| Froude number | `tsr.froude2d` | result | Time-varying |
| Speed | `tsr.SPEED2D` | result | Time-varying; water velocity |
| Unit flow | `tsr.unitflow2d` | result | Time-varying; flow per unit length |
| Volume | `tsr.SGVOLUME2D` | result | Time-varying; subgrid models only |

#### Summary (non time-varying) results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Subgrid flag | `tsr.SUBGRID2D` | result | Summary; 1=subgrid, 2=non-subgrid |
| Direction at first max velocity | `tsr.MAXANGLE2D` | result | Summary |
| Direction at first min velocity | `tsr.MINANGLE2D` | result | Summary; undefined if min velocity is zero |
| Direction at first max hazard | `tsr.MAXHAZANGLE2D` | result | Summary |
| Depth at first max hazard | `tsr.MAXHAZDEPTH2D` | result | Summary |
| Speed at first max hazard | `tsr.MAXHAZSPEED2D` | result | Summary |
| Direction at first max depth | `tsr.MAXDEPTHANGLE2D` | result | Summary |
| Direction at first max velocity above threshold | `tsr.MAXVELDEPTHANGLE2D` | result | Summary |
| Mesh element area | `tsr.AREA2D` | result | Summary |
| Element level (ground level) | `tsr.GNDLEV2D` | result | Summary |
| Max hazard | `tsr.HAZARD2D` | result | Summary; DEFRA HR = d×(v+0.5)+DF |
| Max depth | `tsr.MAX_DEPTH2D` | result | Summary |
| Max elevation | `tsr.MAX_ELEVATION2D` | result | Summary |
| Max speed | `tsr.MAX_SPEED2D` | result | Summary |
| Max unit flow | `tsr.MAXUNITFLOW2D` | result | Summary |
| Min depth | `tsr.MINDEPTH2D` | result | Summary |
| Min speed | `tsr.MINSPEED2D` | result | Summary |
| Min unit flow | `tsr.MINUNITFLOW2D` | result | Summary |
| Rainfall profile | `tsr.RAINPROF2D` | result | Summary |
| Time to last inundation | `tsr.T_END_INUNDATION_2D` | result | Summary; -1 if threshold not met |
| Total inundation duration | `tsr.T_FLOOD_DURATION_2D` | result | Summary; -1 if threshold not met |
| Time to first inundation | `tsr.T_INUNDATION_2D` | result | Summary; -1 if threshold not met |
| Time to peak inundation | `tsr.T_PEAK_2D` | result | Summary; -1 if element dry throughout |
| Volume error | `tsr.VOLERROR2D` | result | Summary |

### Network Results Point Results (2D) (`sw_2d_results_point`)

#### Time-varying hydraulic results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Direction | `tsr.ANGLE2D` | result | Time-varying; flow direction in radians from due East |
| Mean depth | `tsr.AVEDEPTH2D` | result | Time-varying; subgrid models only |
| Depth | `tsr.DEPTH2D` | result | Time-varying; highest depth for subgrid elements |
| Eddy viscosity | `tsr.EDDYVISCOSITY2D` | result | Time-varying |
| Elevation | `tsr.ELEVATION2D` | result | Time-varying |
| Froude number | `tsr.FROUDE2D` | result | Time-varying |
| Speed | `tsr.SPEED2D` | result | Time-varying |
| Unit flow | `tsr.UNITFLOW2D` | result | Time-varying |
| Volume | `tsr.SGVOLUME2D` | result | Time-varying; subgrid models only |

#### Summary (non time-varying) hydraulic results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Element level (ground level) | `tsr.GNDLEV2D` | result | Summary |
| Max mean depth | `tsr.MAXAVEDEPTH2D` | result | Summary; subgrid models only |
| Max depth | `tsr.MAXDEPTH2D` | result | Summary |
| Max eddy viscosity | `tsr.MAXEDDYVISCOSITY2D` | result | Summary |
| Max elevation | `tsr.MAXELEVATION2D` | result | Summary |
| Max speed | `tsr.MAXSPEED2D` | result | Summary |
| Max unit flow | `tsr.MAXUNITFLOW2D` | result | Summary |
| Max volume | `tsr.MAXSGVOLUME2D` | result | Summary; subgrid models only |
| Min mean depth | `tsr.MINAVEDEPTH2D` | result | Summary; subgrid models only |
| Min depth | `tsr.MINDEPTH2D` | result | Summary |
| Min elevation | `tsr.MINELEVATION2D` | result | Summary |
| Min speed | `tsr.MINSPEED2D` | result | Summary |
| Min unit flow | `tsr.MINUNITFLOW2D` | result | Summary |
| Min volume | `tsr.MINSGVOLUME2D` | result | Summary; subgrid models only |
| Direction at first max velocity | `tsr.MAXANGLE2D` | result | Summary |
| Direction at first min velocity | `tsr.MINANGLE2D` | result | Summary; undefined if min velocity is zero |
| Direction at first max hazard | `tsr.MAXHAZANGLE2D` | result | Summary |
| Depth at first max hazard | `tsr.MAXHAZDEPTH2D` | result | Summary |
| Speed at first max hazard | `tsr.MAXHAZSPEED2D` | result | Summary |
| Direction at first max depth | `tsr.MAXDEPTHANGLE2D` | result | Summary |
| Direction at first max velocity above threshold | `tsr.MAXVELDEPTHANGLE2D` | result | Summary |

### Network Results Line Results (2D) (`sw_2d_results_line`)

#### Time-varying hydraulic results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Flow | `tsr.FLOW2D` | result | Time-varying |
| Highest depth on line | `tsr.HIGHDEPTH2D` | result | Time-varying |
| Highest elevation on line | `tsr.HIGHELEVATION2D` | result | Time-varying; null when elements are dry |
| Highest speed normal to line | `tsr.HIGHSPEEDNORMAL2D` | result | Time-varying; absolute value |
| Lowest depth on line | `tsr.LOWDEPTH2D` | result | Time-varying |
| Lowest elevation on line | `tsr.LOWELEVATION2D` | result | Time-varying; null when elements are dry |
| Lowest speed normal to line | `tsr.LOWSPEEDNORMAL2D` | result | Time-varying; absolute value |

#### Summary (non time-varying) hydraulic results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Max flow | `tsr.MAXFLOW2D` | result | Summary |
| Max highest depth on line | `tsr.MAXHIGHDEPTH2D` | result | Summary |
| Max highest elevation on line | `tsr.MAXHIGHELEVATION2D` | result | Summary; null if elements dry throughout |
| Max highest speed normal to line | `tsr.MAXHIGHSPEEDNORMAL2D` | result | Summary |
| Max ground level | `tsr.MAXGNDLEV2D` | result | Summary |
| Min flow | `tsr.MINFLOW2D` | result | Summary |
| Min lowest depth on line | `tsr.MINLOWDEPTH2D` | result | Summary |
| Min lowest elevation on line | `tsr.MINLOWELEVATION2D` | result | Summary |
| Min lowest speed normal to line | `tsr.MINLOWSPEEDNORMAL2D` | result | Summary |
| Min ground level | `tsr.MINGNDLEV2D` | result | Summary |
| Mean ground level | `tsr.AVEGNDLEV2D` | result | Summary |

### Network Results Polygon Results (2D) (`sw_2d_results_polygon`)

#### Time-varying hydraulic results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Flow into polygon | `tsr.FLOW2D` | result | Time-varying; positive = into polygon |
| Highest depth | `tsr.HIGHDEPTH2D` | result | Time-varying |
| Highest elevation | `tsr.HIGHELEVATION2D` | result | Time-varying; null when elements are dry |
| Highest speed | `tsr.HIGHSPEED2D` | result | Time-varying |
| Lowest depth | `tsr.LOWDEPTH2D` | result | Time-varying |
| Lowest elevation | `tsr.LOWELEVATION2D` | result | Time-varying; null when elements are dry |
| Lowest speed | `tsr.LOWSPEED2D` | result | Time-varying |
| Cumulative rainfall | `tsr.RAINDPTH` | result | Time-varying |
| Rainfall | `tsr.RAINFALL` | result | Time-varying; rainfall intensity |
| Enclosed volume | `tsr.VOLUME2D` | result | Time-varying |
| Area flooded to inundation depth | `tsr.FLOODED_AREA2D` | result | Time-varying |

#### Summary (non time-varying) hydraulic results

| UI / Meaning | Database Field | Type | Notes |
|--------------|----------------|------|-------|
| Max flow into polygon | `tsr.MAXFLOW2D` | result | Summary |
| Max highest depth | `tsr.MAXHIGHDEPTH2D` | result | Summary |
| Max highest elevation | `tsr.MAXHIGHELEVATION2D` | result | Summary; null if elements dry throughout |
| Max highest speed | `tsr.MAXHIGHSPEED2D` | result | Summary |
| Max enclosed volume | `tsr.MAXVOLUME2D` | result | Summary |
| Max ground level | `tsr.MAXGNDLEV2D` | result | Summary |
| Max area flooded to inundation depth | `tsr.MAXFLOODED_AREA2D` | result | Summary |
| Min flow into polygon | `tsr.MINFLOW2D` | result | Summary |
| Min lowest depth | `tsr.MINLOWDEPTH2D` | result | Summary |
| Min lowest elevation | `tsr.MINLOWELEVATION2D` | result | Summary |
| Min lowest speed | `tsr.MINLOWSPEED2D` | result | Summary |
| Min enclosed volume | `tsr.MINVOLUME2D` | result | Summary |
| Min ground level | `tsr.MINGNDLEV2D` | result | Summary |
| Min area flooded to inundation depth | `tsr.MINFLOODED_AREA2D` | result | Summary |
| Mean ground level | `tsr.AVEGNDLEV2D` | result | Summary |
| Total area flooded to inundation depth | `tsr.TOTAL_FLOODED_AREA2D` | result | Summary |
