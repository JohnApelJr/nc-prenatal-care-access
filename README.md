# NC Pediatric Access — Travel-Time Pipeline
**Time-to-Care | Western NC Pilot | Phase 2**

Calculates drive-time from Census block group centroids to nearest pediatric specialist  
by specialty across 18 WNC counties. Extends the Phase 1 NPI provider supply analysis  
with actual routing distances to quantify functional access gaps.

---

## Outputs

| Output | Description |
|--------|-------------|
| Drive-time maps | Interactive Folium map per specialty showing access tiers |
| Composite burden map | All-specialty combined burden choropleth with top-priority markers |
| County summary table | Planner-ready CSV with median drive times and unreachable child counts |
| Priority community list | Top 25 block groups ranked by access burden score (children × drive time) |

---

## Setup

```bash
git clone https://github.com/JohnApelJr/nc-travel-time-pipeline.git
cd nc-travel-time-pipeline
pip install pandas numpy geopandas folium mapclassify matplotlib requests uszipcode
```

### API Keys Required
- **OpenRouteService** (free): https://openrouteservice.org/dev/#/signup  
  Free tier: 2,000 matrix requests/day — sufficient for 18-county WNC pilot
- **Census API** (free): https://api.census.gov/data/key_signup.html

Set as environment variables:
```bash
export ORS_API_KEY=your_key_here
export CENSUS_API_KEY=your_key_here
```

---

## Data Requirements

Place your Phase 1 NPI pipeline output at `data/raw/npi_providers_nc.csv`  
with the following columns:

| Column | Description |
|--------|-------------|
| `npi` | National Provider Identifier |
| `provider_name` | Provider name |
| `specialty` | Specialty label (must match SPECIALTIES list in config) |
| `zip_code` | 5-digit ZIP code |
| `county_fips` | 5-digit county FIPS |

---

## Access Tier Classification

| Tier | Drive Time |
|------|------------|
| Accessible | ≤ 30 minutes |
| Burdened | 31–60 minutes |
| Significantly Burdened | 61–90 minutes |
| Effectively Unreachable | > 90 minutes |
| No Provider | No provider found in NC |

---

## Priority Score

`access_burden_score = child_population × (mean_drive_minutes / 30)`

Higher score = more children with longer drives = higher planning priority  
for mobile clinic siting, telehealth hub placement, or grant applications.

---

## Specialties (Phase 2)

Pediatric Neurology · PT/OT/SLP · Developmental Pediatrics · Pediatric Cardiology  
Behavioral Health · Audiology · ENT · Vision

*GI and Nutrition reserved for Time-to-Care platform build*

---

## Methodology Notes

- Provider locations derived from ZIP centroids (~2–5 mi positional error)
- ~10–20 min routing error possible in WNC mountain terrain
- Flag as limitation; Phase 3 upgrade: geocode full street addresses
- Drive-time assumes average speeds; no traffic or seasonal conditions

---

## Repository Structure

```
nc-travel-time-pipeline/
├── notebooks/
│   └── travel_time_pipeline.ipynb
├── data/
│   ├── raw/           ← Place npi_providers_nc.csv here
│   └── processed/     ← Generated: OD matrix, classified GeoPackage
├── figures/
├── maps/              ← Generated: HTML interactive maps
├── outputs/           ← Generated: CSV summary tables
└── README.md
```

---

## Author
John Apel · M.S. Applied Data Science — Syracuse University  
[Portfolio](https://johnapeljr.github.io) · [LinkedIn](https://linkedin.com/in/johnapeljr) · [GitHub](https://github.com/JohnApelJr)
