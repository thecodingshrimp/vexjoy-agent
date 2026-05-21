# Seasonal Logic

> **Scope**: USDA hardiness zone lookup, frost date estimation, indoor-start calculation formulas, and transition rules.
> **Load when**: calculating frost dates, indoor start dates, transplant windows, or determining zone from location.

---

## USDA Hardiness Zone Lookup (by Country/Region)

### Netherlands (primary locale for this agent)

The Netherlands falls primarily in USDA zones 8a–9a. Use this table:

| Region | USDA Zone | Avg Last Frost | Avg First Frost |
|--------|-----------|----------------|-----------------|
| North (Groningen, Friesland, Drenthe) | 8a | April 15 | October 31 |
| East (Overijssel, Gelderland) | 8a–8b | April 10 | November 1 |
| West (Noord-Holland, Zuid-Holland, Zeeland) | 8b–9a | March 31 | November 10 |
| South (Noord-Brabant, Limburg) | 8b | April 5 | November 5 |
| Central (Utrecht, Flevoland) | 8a–8b | April 8 | November 3 |
| Amsterdam | 8b | April 1 | November 8 |
| Rotterdam | 9a | March 28 | November 12 |
| The Hague / Den Haag | 8b–9a | April 1 | November 10 |

**Default for Netherlands without city**: use Zone 8b, last frost April 5, first frost November 5.

### United States (reference zones)

| Zone | Avg Min Winter Temp (°F) | Example Cities | Avg Last Frost | Avg First Frost |
|------|------------------------|----------------|----------------|-----------------|
| 3b | -35 to -30 | Fargo ND, Duluth MN | May 15 | Sept 15 |
| 4a | -30 to -25 | Minneapolis MN | May 1 | Oct 1 |
| 4b | -25 to -20 | Denver CO, Madison WI | April 25 | Oct 5 |
| 5a | -20 to -15 | Chicago IL, Kansas City MO | April 15 | Oct 15 |
| 5b | -15 to -10 | Columbus OH, St. Louis MO | April 10 | Oct 20 |
| 6a | -10 to -5 | Philadelphia PA, Louisville KY | April 1 | Oct 30 |
| 6b | -5 to 0 | Baltimore MD, Nashville TN | March 25 | Nov 5 |
| 7a | 0 to 5 | Oklahoma City OK, Richmond VA | March 15 | Nov 15 |
| 7b | 5 to 10 | Dallas TX, Charlotte NC | March 5 | Nov 20 |
| 8a | 10 to 15 | Seattle WA, Atlanta GA | Feb 25 | Nov 28 |
| 8b | 15 to 20 | Portland OR, San Antonio TX | Feb 15 | Dec 5 |
| 9a | 20 to 25 | Los Angeles CA, Sacramento CA | Feb 1 | Dec 15 |
| 9b | 25 to 30 | Phoenix AZ, Miami FL | Jan 15 | Dec 28 |
| 10a | 30 to 35 | Miami FL, Honolulu HI | Jan 1 | No frost |
| 11+ | 40+ | Tropics | No frost | No frost |

### UK (reference)

| Region | USDA Zone | Avg Last Frost | Avg First Frost |
|--------|-----------|----------------|-----------------|
| Scotland highlands | 7a | April 20 | Oct 20 |
| Scotland lowlands | 8a | April 10 | Oct 30 |
| Northern England | 8a | April 5 | Nov 1 |
| Midlands | 8b | March 31 | Nov 5 |
| South England | 9a | March 20 | Nov 15 |
| London | 9a | March 18 | Nov 18 |

---

## Date Calculation Formulas

### Indoor Start Date

```
indoor_start_date = last_frost_date - (indoor_weeks * 7)
```

Example: Last frost April 5, tomato needs 6–8 wks indoors.
- Early start: April 5 - 56 days = February 8
- Late start: April 5 - 42 days = February 22
- Recommended: February 15 (7 wks)

### Transplant Date

```
transplant_date = last_frost_date + (weeks_after_frost * 7)
```

For frost-sensitive crops (tomato, pepper, basil): last_frost + 14 days minimum.
For frost-tolerant crops (lettuce, kale, broccoli): last_frost - 21 days (transplant before last frost).

### Harvest Start Date (Optimistic)

```
harvest_start = transplant_date + days_to_maturity
```

### Harvest Start Date (Conservative, recommended)

```
harvest_start = transplant_date + (days_to_maturity * 1.15)
```

Add 15% buffer for slower growth in containers, suboptimal weather, or establishment lag.

---

## Growing Season Duration

```
growing_season_days = first_frost_date - last_frost_date
```

| Zone | Typical Growing Season |
|------|----------------------|
| 3b | 100–120 days |
| 4a–4b | 120–140 days |
| 5a–5b | 140–160 days |
| 6a–6b | 160–180 days |
| 7a–7b | 180–210 days |
| 8a–8b | 210–240 days |
| 9a–9b | 240–300 days |
| 10+ | Year-round |

---

## Indoor-to-Outdoor Transition Rules

### Hardening Off Protocol (mandatory for all transplants)

Plants started indoors need gradual outdoor exposure before transplanting. Budget 7–14 days.

| Day | Exposure |
|-----|----------|
| 1–2 | 1–2 hrs shade, sheltered from wind |
| 3–4 | 2–3 hrs indirect sun |
| 5–6 | 3–4 hrs morning sun |
| 7–8 | Half day full sun |
| 9–10 | Full day, bring in at night |
| 11–14 | Leave out overnight if no frost |

### Soil Temperature Requirements

| Crop | Min Soil Temp to Transplant |
|------|---------------------------|
| Tomato, pepper, eggplant | 60°F / 16°C |
| Cucumber, squash, bean | 60°F / 16°C |
| Lettuce, spinach, kale | 40°F / 4°C |
| Carrot, beet, pea (direct sow) | 40°F / 4°C |
| Basil | 65°F / 18°C |

**Netherlands reference**: soil at 10cm depth typically reaches 60°F around late April–early May in Zone 8b.

---

## Fall Planting Windows

For cool-season crops planted in late summer for fall harvest:

```
fall_sow_date = first_frost_date - days_to_maturity - 14
```

Example: First frost November 5, lettuce 50 days to maturity.
- Nov 5 - 50 days - 14 days buffer = September 2
- Sow lettuce indoors or direct-sow by early September for fall harvest.

| Crop | Latest Sow Date Before First Frost |
|------|----------------------------------|
| Radish | First frost - 25 days |
| Lettuce | First frost - 55 days |
| Spinach | First frost - 50 days |
| Kale | First frost - 55 days (kale tolerates light frost) |
| Arugula | First frost - 40 days |

---

## Netherlands-Specific Calendar (Zone 8b Default)

| Month | Activity |
|-------|----------|
| January | Order seeds; plan inventory needs |
| February | Start tomatoes, peppers, eggplant, onions indoors (under lights) |
| March | Start cucumbers, squash, basil indoors; harden off brassicas |
| Early April | Transplant brassicas, lettuce, peas outdoors; direct sow carrots, beets |
| Late April | Harden off tomatoes, peppers; transplant cucumbers after last frost risk |
| May 1–15 | Transplant tomatoes, peppers (watch forecast; last frost risk ~April 5) |
| May–June | Direct sow beans, nasturtium, marigold; plant out basil |
| June–August | Succession sow lettuce, radish, cilantro every 3 weeks |
| August | Start fall brassicas indoors; sow fall spinach, lettuce outdoors |
| September | Harvest summer crops; transplant fall brassica seedlings |
| October–November | Harvest remaining crops; plant garlic cloves |
| December | Clean and inventory containers, tools; store tender bulbs |
