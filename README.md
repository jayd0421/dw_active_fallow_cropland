# Enhancing Dynamic World for Active & Fallow Cropland Mapping

This repository contains a Jupyter Notebook (`active_fallow_cropland.ipynb`) that implements a **Google Earth Engine (GEE)** workflow to distinguish between:

- **Active cropland**: fields currently under cultivation (growing)
- **Fallow cropland**: fields not currently cultivated, but with evidence of sustained cultivation in the recent past

The approach is designed to support **food security and humanitarian assessments**, where knowing whether land is actively producing (or temporarily idle) matters for early warning and response planning.

---

## What the notebook does

### Core idea
Using **Dynamic World (GOOGLE/DYNAMICWORLD/V1)**, the notebook:
1. Builds a **monthly modal label** time series (per month, take the **mode** of all DW `label` observations).
2. Derives a **potential cropland mask** from a long historical window using a **sliding-window rule**:
   - A pixel is “potential cropland” if it was labelled as cropland in at least `min_crop_months` within any `crop_window` months.
3. Classifies pixels inside the potential cropland mask as:
   - **Active** (enough cropland months in the current window)
   - **Fallow** (not active now, but historically sustained cropland)
4. Computes **duration layers**:
   - **Active duration**: crop-month count over the last `min_active_duration` months (0–12 by default)
   - **Fallow duration**: months since last sustained cropland (with a visualisation cap for stability)

---

## Outputs

The notebook produces (as Earth Engine images/layers):
- **Active vs Fallow classification**
- **Active cropland duration** (months)
- **Fallow duration** (months since last sustained crop)
- Optional visual inspection layers (Dynamic World/Sentinel-2 context in the map)

---

## Requirements

- Python 3.9+ recommended
- A Google Earth Engine account enabled for the email you authenticate with
- Packages:
  - `earthengine-api`
  - `geemap`

Install (if needed):
```bash
pip install earthengine-api geemap
