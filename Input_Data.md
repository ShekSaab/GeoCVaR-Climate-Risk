# Data Directory

## Required Input

Place your input file here:

**`Geo CVaR Input Data Contract.xlsx`**

This Excel file must contain three sheets:

### Sheet 1: Asset Inventory

| Column | Type | Description |
|--------|------|-------------|
| `asset_id` | int | Unique asset identifier |
| `country` | str | Country name |
| `country_code` | str | ISO country code |
| `region` | str | Geographic region |
| `primary_fuel` | str | Fuel type (Coal, Gas, Solar, Wind, etc.) |
| `capacity_mw` | float | Generation capacity in MW |
| `asset_value_mln_usd` | float | Asset value in millions USD |
| `latitude` | float | Latitude coordinate |
| `longitude` | float | Longitude coordinate |

### Sheet 2: Vulnerability Rules

| Column | Type | Description |
|--------|------|-------------|
| `asset_type` | str | Asset category (e.g., ENERGY) |
| `primary_fuel` | str | Fuel type (join key to Asset Inventory) |
| `hazard_type` | str | Climate hazard (Heat, Flood, Drought, Storm) |
| `vulnerability_score` | float | Base vulnerability score [0, 1] |

### Sheet 3: Scenario Settings

| Column | Type | Description |
|--------|------|-------------|
| `scenario` | str | Scenario name (ORDERLY, CURRENT, HOT) |
| `description` | str | Scenario description |
| `temp_increases_in_C` | float | Temperature increase in degrees Celsius |

## Note

The data file is excluded from version control via `.gitignore` to avoid
distributing potentially sensitive portfolio data. Contact the repository
owner for sample data or use your own data matching the schema above.
