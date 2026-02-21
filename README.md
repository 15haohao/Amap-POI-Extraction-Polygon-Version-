# Amap POI Extraction (Rectangle Splitting)

This repo contains a single Jupyter notebook (`mapping-square.ipynb`) that batch-extracts POIs from the Amap (Gaode) Web Service **polygon** API within one or multiple **rectangular bounding boxes**, and exports results to CSV.

## What it does
- Input: one or more rectangles in the form `[min_lon, max_lat, max_lon, min_lat]` (lon first, lat second)
- Query: Amap Place Polygon API (`/v3/place/polygon`)
- Output: a CSV file with columns `name, address, adname, lng, lat`
  - `lng/lat` are converted to **WGS84** (Amap returns **GCJ-02**; conversion is applied)

## Requirements
- Python 3
- `requests`
- A coordinate conversion helper: `Coordin_transformlat.py` providing `gcj02towgs84`

Install dependency:
```bash
pip install requests
```

## Setup
You need an **Amap Web Service Key**. Do **not** hardcode keys in public repos.

Set the key via environment variable `AMAP_KEY`:

**Windows PowerShell**
```powershell
$env:AMAP_KEY="YOUR_KEY"
```

**macOS/Linux**
```bash
export AMAP_KEY="YOUR_KEY"
```

## How to run
1. Open `mapping-square.ipynb`
2. Run the **Config** cell and edit:
   - `POI_TYPES` (Amap category code, e.g. `060000` for catering)
   - `INPUT_RECTS` (list of rectangles)
   - `OUTPUT_CSV` (output filename/path)
   - Optional throttling/splitting params: `COUNT_LIMIT`, `REQUEST_TIMEOUT`, `REQUEST_SLEEP`
3. Run the **Dependencies & Functions** cell (once)
4. Run the **Execution** cell to start extraction
5. (Optional) Run the **Read Results** cell to validate output

## Notes
- Rate limiting: if requests fail due to throttling, increase `REQUEST_SLEEP`.
- The notebook appends to the CSV by default; delete the old CSV if you want a clean rerun.
- Please comply with Amap terms of service and use data only for authorised / research purposes.
