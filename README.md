# DBbun GBIF Aquatic Biodiversity Early-Warning Simulator

An open, reproducible Jupyter notebook that converts a GBIF-mediated aquatic occurrence
download into a structured early-warning and planning tool.

**Submitted to:** 2026 GBIF Ebbe Nielsen Challenge  
**Author:** Uri Kartoun, DBbun LLC, Cambridge MA, USA  
**License:** MIT

---

## What it does

Starting from a GBIF Simple CSV download, the simulator produces:

| Output | Description |
|--------|-------------|
| `column_quality_profile.csv` | Column-level data quality profile |
| `top_species.csv` | Top 30 species by record count |
| `species_year_counts.csv` | Species × year occurrence matrix |
| `emerging_species_signals.csv` | Effort-corrected emerging signal scores (2,536 species) |
| `dbbun_priority_monitoring_table.csv` | Composite risk-ranked top 25 priority species |
| `spatial_hotspot_grid.csv` | 0.5° × 0.5° grid cell hotspot scores |
| `synthetic_future_occurrence_pressure_scenarios.csv` | Synthetic 2026–2030 scenario projections |
| `trajectory_clusters_pca.png` | PCA plot of 5 species trajectory clusters |
| `spatial_hotspot_map.png` | Norway aquatic biodiversity risk hotspot map |
| `scenario_comparison_2026_2030.png` | 5-scenario occurrence pressure comparison |
| `scenario_heatmap_2030.png` | Species × scenario heatmap (2030) |
| `DBbun_GBIF_Early_Warning_Report.md` | Auto-generated structured Markdown report |

---

## How to run

### Requirements

```
python >= 3.9
pandas
numpy
matplotlib
scikit-learn
```

Install dependencies:

```bash
pip install pandas numpy matplotlib scikit-learn
```

### Setup

```
your-repo/
├── DBbun_GBIF_Aquatic_Biodiversity_Early_Warning_Simulator.ipynb
├── data/
│   └── 0032755-260519110011954.csv   ← your GBIF Simple CSV download
└── outputs/                          ← created automatically on first run
```

### Run

Open the notebook and run all cells top-to-bottom (`Kernel → Restart & Run All`).

To use with a different GBIF dataset, update `DATA_PATH` in cell 2:

```python
DATA_PATH = Path('data/your-gbif-download.csv')
```

The notebook auto-detects tab vs. comma delimiters and selects available columns — it works
with any GBIF Simple occurrence download.

---

## How it works

The workflow has two parts:

**Part 1 — Exploration and signal scoring**
- Load and profile a GBIF Simple CSV
- Clean coordinates, years, and occurrence status
- Compute survey-effort-corrected emerging signal scores
- Apply a minimum baseline filter (≥3 survey years before 2021) to remove data-gap artifacts
- Export initial outputs

**Part 2 — Machine learning early-warning engine**
- Build a species feature matrix (growth, spread, trajectory, anomaly)
- **Isolation Forest** anomaly detection (200 trees, 8% contamination)
- **KMeans** trajectory clustering into 5 classes: Emerging, Stable, Declining, Bursty, Sparse
- Composite risk score (0–100) combining growth, spatial spread, anomaly, and alien flag
- Spatial hotspot detection on a 0.5° grid
- **Scenario simulator**: 5 planning scenarios × 5 years × top 50 species
- Auto-generated Markdown report

---

## Dataset used in this submission

**Vannmiljø - artsforekomster**  
Published by the Norwegian Environment Agency (Miljødirektoratet), hosted by GBIF Norway.  
DOI: [10.15468/u35j52](https://doi.org/10.15468/u35j52)  
Records: 1,951,094 | Species: 4,563 | Years: 2008–2026  
Licence: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

> The source data is licensed CC BY 4.0. The notebook code is licensed MIT.
> When reusing outputs derived from this dataset, please cite the dataset DOI above.

---

## Key results (run on Vannmiljø dataset)

- **2,536 species** retained after effort-correction and baseline filter
- **68 Emerging** trajectory species identified
- **203 anomalous** occurrence patterns flagged
- **13 candidate** management-relevant taxa cross-referenced
- **Top hotspot:** Trondheim Fjord region (63.5°N, 9.0°E) — 1,215 species, 27,520 records
- **Scenario range (2030):** Rapid Intervention −99% vs. Increased Boating +662% vs. Baseline

---

## Scenario descriptions

| Scenario | Growth multiplier | Interpretation |
|----------|------------------|----------------|
| Baseline | ×1.0 | Current monitoring, no change in pressure |
| Warming Waters | ×1.3 | +2°C effect on cold-adapted aquatic species |
| Increased Boating | ×1.5 | Hull fouling and ballast water dispersal increase |
| Early Detection and Response | ×0.7 | Monitoring effort doubles with rapid containment |
| Rapid Intervention | ×0.4 | Active removal and biosecurity in top hotspots |

> **Note:** Scenario outputs are planning simulations, not ecological forecasts.

---

## Candidate management-relevant taxa

The notebook includes an editable list of aquatic management-relevant taxa drawn from NOBANIS
and Artsdatabanken. This is a screening aid, not an authoritative registry. Users should verify
matches against their national alien species database before drawing management conclusions.

---

## Citation

Kartoun, U. (2026). DBbun GBIF Aquatic Biodiversity Early-Warning Simulator.
2026 GBIF Ebbe Nielsen Challenge submission. DBbun LLC, Cambridge MA.
