# Flexible Schema Sample Data

Use `free_format_laminator_timeseries_en.csv` first when testing the dashboard.

Recommended English columns:

- `timestamp`: event or sample time
- `equipment_id`: laminator equipment id
- `line`: production line
- `defect_code`: defect type such as `CRACK` or `BUBBLE`
- `judge`: inspection result such as `NG`
- `vacuum_kpa`: vacuum profile value
- `upper_temp_c`: temperature profile value
- `pressure_kpa`: pressure profile value
- `process_time_sec`: process or cycle time
- `recipe_id`: recipe condition
- `bom_material`: BOM/material condition
- `image_ref`: EL/AOI image reference id
- `note`: manual note

The dashboard also recognizes Korean column names, but English headers are safer for Excel, GitHub, and cross-PC sharing.
