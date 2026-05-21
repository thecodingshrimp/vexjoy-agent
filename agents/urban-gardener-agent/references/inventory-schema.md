# Inventory Schema

> **Scope**: Excel workbook sheet layouts, column definitions, and openpyxl script templates for garden inventory, planting schedules, harvest calendars, and treatment logs.
> **Load when**: generating any Excel or CSV file for garden management.

---

## Workbook Structure

Every generated workbook contains these tabs in order:

1. **Inventory** — physical supplies (pots, soil, fertilizer, tools, seeds)
2. **Planting Schedule** — what to plant, when, and where
3. **Harvest Calendar** — expected harvest dates and yield estimates
4. **Treatment Log** — watering, fertilizing, pest, pruning records
5. **README** — column definitions, zone, frost dates, generation date

---

## Tab 1: Inventory

Tracks all physical gardening supplies.

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| Category | Text | Supply category | Pot, Soil, Fertilizer, Tool, Seed |
| Item Name | Text | Specific item | "Terracotta pot 30cm", "Perlite 10L" |
| Quantity | Number | Amount on hand | 5 |
| Unit | Text | Unit of measure | pcs, L, kg, bag, packet |
| Size / Spec | Text | Dimensions or specs | "30cm diameter, 8L", "5-10-5 NPK" |
| Status | Dropdown | Current status | Available, In Use, Empty, Needs Reorder |
| Location | Text | Where stored/used | "Balcony shelf", "Storage room" |
| Notes | Text | Free notes | "Crack in bottom — still usable" |
| Assigned To | Text | Plant using this item | "Tomato (Roma)" |

### Inventory Categories

**Pots & Containers**
- Terracotta pots (by diameter: 10cm, 15cm, 20cm, 25cm, 30cm, 40cm+)
- Plastic grow pots (by volume: 1L, 3L, 5L, 10L, 15L, 20L+)
- Fabric grow bags (by gallon: 1, 3, 5, 7, 10, 15, 20, 25, 30)
- Window boxes (by length: 40cm, 60cm, 80cm, 100cm)
- Hanging baskets
- Seed trays (cell count: 6, 12, 24, 40, 72, 128)
- Propagation trays + domes

**Soil & Growing Media**
- Potting mix (all-purpose)
- Seed starting mix
- Perlite
- Vermiculite
- Coco coir
- Compost (homemade or bagged)
- Bark mulch
- Sand (horticultural)

**Fertilizers**
- Slow-release granular (NPK ratio)
- Liquid all-purpose (NPK ratio)
- Tomato-specific fertilizer
- Calcium/magnesium supplement
- Seaweed / kelp extract
- Worm castings

**Tools**
- Hand trowel
- Cultivator / hand fork
- Watering can (volume)
- Garden hose + nozzle
- Spray bottle (manual / pump)
- Pruning shears / secateurs
- Plant labels / markers
- String / plant ties
- Stakes (wooden, bamboo — by length)
- Cages (tomato, pepper)
- Trellises / netting
- Grow light (type, wattage)
- Heat mat
- Thermometer / hygrometer

**Seeds**
- Plant name
- Variety
- Packet count
- Approximate seed count
- Expiry / packed-for year
- Source (brand or supplier)

---

## Tab 2: Planting Schedule

Tracks the full lifecycle from seed to transplant.

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| Plant Name | Text | Common name | Tomato |
| Variety | Text | Cultivar name | "Roma VF", "Sungold F1" |
| Indoor Start Date | Date | When to sow indoors | 2026-02-15 |
| Germination Expected | Date | Indoor start + germ days | 2026-02-22 |
| Pot-Up Date | Date | Move to larger pot indoors | 2026-03-15 |
| Harden Off Start | Date | Begin outdoor exposure | 2026-04-20 |
| Transplant Date | Date | Move outdoors permanently | 2026-05-01 |
| Container / Location | Text | Final growing spot | "5-gal fabric bag, balcony south" |
| Status | Dropdown | Current lifecycle stage | Planned, Sown, Germinated, Potted Up, Hardening, Transplanted, Harvesting, Done |
| Seeds Sown | Number | Count of seeds started | 6 |
| Seedlings Kept | Number | Survivors after thinning | 2 |
| Notes | Text | Observations | "Germinated in 6 days" |

---

## Tab 3: Harvest Calendar

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| Plant Name | Text | Common name | Tomato |
| Variety | Text | Cultivar | "Sungold F1" |
| Transplant Date | Date | From Planting Schedule | 2026-05-01 |
| Harvest Start (Optimistic) | Date | Transplant + DTM | 2026-07-15 |
| Harvest Start (Conservative) | Date | Transplant + DTM × 1.15 | 2026-07-25 |
| Harvest End (Expected) | Date | Based on frost date | 2026-10-31 |
| Est. Yield Per Plant | Text | Range | "1–2 kg/week at peak" |
| Harvest Count | Number | Actual harvests logged | 12 |
| Total Yield (kg) | Number | Running total | 4.5 |
| Notes | Text | Quality, issues | "Blossom end rot on 3 fruits" |

---

## Tab 4: Treatment Log

Running log of all care actions.

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| Date | Date | When action taken | 2026-06-10 |
| Plant Name | Text | Which plant | Tomato |
| Action | Dropdown | Type of care | Water, Fertilize, Prune, Spray, Repot, Stake, Other |
| Product | Text | What was used | "Tomato focus NPK 4-4.5-8" |
| Dose / Amount | Text | Quantity applied | "20ml per 5L water" |
| Method | Text | How applied | Watering can, spray bottle, top-dress |
| Result / Notes | Text | Observation after | "Yellowing leaves improved after 1 wk" |

---

## openpyxl Script Template

Use this template to generate the workbook. Fill in the data arrays before running.

```python
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.utils import get_column_letter
from datetime import date, datetime
import os

# ── Configuration ────────────────────────────────────────────
ZONE = "8b"
LAST_FROST = date(2026, 4, 5)
FIRST_FROST = date(2026, 11, 5)
OUTPUT_FILE = os.path.expanduser("~/garden_plan_2026.xlsx")

# ── Style helpers ────────────────────────────────────────────
HEADER_FILL = PatternFill("solid", fgColor="2E7D32")   # dark green
HEADER_FONT = Font(color="FFFFFF", bold=True)
ALT_FILL    = PatternFill("solid", fgColor="E8F5E9")   # light green
THIN = Side(style="thin")
BORDER = Border(left=THIN, right=THIN, top=THIN, bottom=THIN)

def style_header_row(ws, col_count):
    for col in range(1, col_count + 1):
        cell = ws.cell(1, col)
        cell.fill = HEADER_FILL
        cell.font = HEADER_FONT
        cell.alignment = Alignment(horizontal="center", wrap_text=True)
        cell.border = BORDER

def style_data_rows(ws, row_count, col_count):
    for row in range(2, row_count + 2):
        fill = ALT_FILL if row % 2 == 0 else PatternFill()
        for col in range(1, col_count + 1):
            cell = ws.cell(row, col)
            cell.fill = fill
            cell.border = BORDER
            cell.alignment = Alignment(wrap_text=True)

def auto_width(ws):
    for col in ws.columns:
        max_len = max((len(str(c.value or "")) for c in col), default=8)
        ws.column_dimensions[get_column_letter(col[0].column)].width = min(max_len + 4, 40)

wb = openpyxl.Workbook()

# ── Tab 1: Inventory ─────────────────────────────────────────
ws_inv = wb.active
ws_inv.title = "Inventory"
inv_headers = ["Category", "Item Name", "Quantity", "Unit",
               "Size / Spec", "Status", "Location", "Notes", "Assigned To"]
ws_inv.append(inv_headers)

# INSERT INVENTORY DATA HERE — list of lists matching header order
inventory_data = [
    # ["Pot", "Terracotta 30cm", 3, "pcs", "30cm dia 8L", "Available", "Balcony shelf", "", ""],
]
for row in inventory_data:
    ws_inv.append(row)

style_header_row(ws_inv, len(inv_headers))
style_data_rows(ws_inv, len(inventory_data), len(inv_headers))
auto_width(ws_inv)

# ── Tab 2: Planting Schedule ─────────────────────────────────
ws_plant = wb.create_sheet("Planting Schedule")
plant_headers = ["Plant Name", "Variety", "Indoor Start", "Germ Expected",
                 "Pot-Up Date", "Harden Off Start", "Transplant Date",
                 "Container / Location", "Status", "Seeds Sown", "Seedlings Kept", "Notes"]
ws_plant.append(plant_headers)

# INSERT PLANTING DATA HERE
planting_data = [
    # ["Tomato", "Sungold F1", date(2026,2,15), date(2026,2,22),
    #  date(2026,3,15), date(2026,4,20), date(2026,5,1),
    #  "5-gal fabric bag south balcony", "Planned", 6, 2, ""],
]
for row in planting_data:
    ws_plant.append(row)

style_header_row(ws_plant, len(plant_headers))
style_data_rows(ws_plant, len(planting_data), len(plant_headers))
auto_width(ws_plant)

# ── Tab 3: Harvest Calendar ───────────────────────────────────
ws_harv = wb.create_sheet("Harvest Calendar")
harv_headers = ["Plant Name", "Variety", "Transplant Date",
                "Harvest Start (Opt)", "Harvest Start (Cons)", "Harvest End",
                "Est. Yield / Plant", "Harvest Count", "Total Yield (kg)", "Notes"]
ws_harv.append(harv_headers)

# INSERT HARVEST DATA HERE
harvest_data = []
for row in harvest_data:
    ws_harv.append(row)

style_header_row(ws_harv, len(harv_headers))
style_data_rows(ws_harv, len(harvest_data), len(harv_headers))
auto_width(ws_harv)

# ── Tab 4: Treatment Log ──────────────────────────────────────
ws_treat = wb.create_sheet("Treatment Log")
treat_headers = ["Date", "Plant Name", "Action", "Product",
                 "Dose / Amount", "Method", "Result / Notes"]
ws_treat.append(treat_headers)

style_header_row(ws_treat, len(treat_headers))
auto_width(ws_treat)

# ── Tab 5: README ─────────────────────────────────────────────
ws_readme = wb.create_sheet("README")
readme_lines = [
    ["Garden Plan — README"],
    [""],
    ["Generated", str(date.today())],
    ["USDA Zone", ZONE],
    ["Last Frost Date (est.)", str(LAST_FROST)],
    ["First Frost Date (est.)", str(FIRST_FROST)],
    [""],
    ["TAB: Inventory"],
    ["Category", "Type of supply: Pot, Soil, Fertilizer, Tool, Seed"],
    ["Status", "Available / In Use / Empty / Needs Reorder"],
    [""],
    ["TAB: Planting Schedule"],
    ["Status", "Planned > Sown > Germinated > Potted Up > Hardening > Transplanted > Harvesting > Done"],
    ["Indoor Start", "Date to sow seeds indoors under lights or in warm location"],
    ["Transplant Date", "Date to move seedling outdoors permanently (after hardening off)"],
    [""],
    ["TAB: Harvest Calendar"],
    ["Harvest Start (Opt)", "Transplant date + days-to-maturity (best case)"],
    ["Harvest Start (Cons)", "Transplant date + days-to-maturity x 1.15 (realistic)"],
    [""],
    ["TAB: Treatment Log"],
    ["Action", "Water / Fertilize / Prune / Spray / Repot / Stake / Other"],
    ["", "Log every treatment with date and product for traceability"],
]
for line in readme_lines:
    ws_readme.append(line)

ws_readme.cell(1, 1).font = Font(bold=True, size=14)
auto_width(ws_readme)

# ── Save ──────────────────────────────────────────────────────
wb.save(OUTPUT_FILE)
print(f"Workbook saved: {OUTPUT_FILE}")
```

---

## CSV Fallback (when openpyxl unavailable)

Generate separate CSV files:

```python
import csv, os
from datetime import date

base = os.path.expanduser("~/garden_plan_2026")
os.makedirs(base, exist_ok=True)

files = {
    "inventory.csv": ["Category","Item Name","Quantity","Unit","Size/Spec","Status","Location","Notes","Assigned To"],
    "planting_schedule.csv": ["Plant Name","Variety","Indoor Start","Transplant Date","Container/Location","Status","Notes"],
    "harvest_calendar.csv": ["Plant Name","Variety","Transplant Date","Harvest Start","Harvest End","Est Yield","Notes"],
    "treatment_log.csv": ["Date","Plant Name","Action","Product","Dose","Method","Notes"],
}

for filename, headers in files.items():
    path = os.path.join(base, filename)
    with open(path, "w", newline="") as f:
        writer = csv.writer(f)
        writer.writerow(headers)
    print(f"Created: {path}")
```
