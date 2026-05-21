# Data Schemas

Use this reference when creating or checking CSV files.

## Production Summary

Required columns:

- `work_date`
- `time_bucket`
- `line`
- `equipment_id`
- `product_model`
- `input_qty`
- `output_qty`
- `inspection_qty`
- `ng_qty`
- `rejudge_qty`
- `reinput_qty`
- `top_defect_code`
- `top_defect_name`

## Defect Events

Required columns:

- `event_time`
- `line`
- `equipment_id`
- `module_id`
- `product_model`
- `defect_code`
- `defect_name`
- `inspection_tool`
- `judge`
- `rejudge_flag`
- `reinput_flag`
- `image_ref`

## Equipment Alarm

Required columns:

- `alarm_time`
- `line`
- `equipment_id`
- `alarm_code`
- `alarm_name`
- `severity`
- `duration_sec`
- `ack_user`

## Laminator Profile

Required columns:

- `sample_time`
- `line`
- `equipment_id`
- `recipe_id`
- `upper_temp_c`
- `lower_temp_c`
- `vacuum_kpa`
- `press_pressure_mpa`
- `belt_speed_mm_s`
- `cycle_time_sec`
- `profile_status`

## Test Scenario Pattern

For dashboard upload testing, include:

- `LAM-03` 14:00 hour with CRACK concentration.
- `LAM-02` 16:00 hour with BUBBLE concentration.
- Matching alarm records around those hours.
- Profile abnormal values around those hours.
- At least one output/inspection mismatch for validation testing.
