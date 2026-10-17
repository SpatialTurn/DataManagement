---
title: "Organizing, Versioning and Backing Up"
teaching: 30
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

- How should I organize the folders of a geospatial project?
- What makes a good file name?
- How do I keep track of versions without losing work?
- How should I store and back up my data?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Set up a clear project folder structure that separates raw, processed and output data
- Apply file naming conventions that sort well and work on every computer
- Keep QGIS projects portable with relative paths
- Choose a versioning approach and apply the 3-2-1 backup rule

::::::::::::::::::::::::::::::::::::::::::::::::

## A place for everything

A consistent folder structure means you, your collaborators and your future self can find things without
asking. One widely used layout for a geospatial project:

```
grocery_access_2026/
├── README.md                  what the project is, who made it, how the folders are organized
├── data/
│   ├── raw/                   original downloads and field exports; never edited
│   ├── processed/             cleaned, reprojected and joined data
│   └── documentation/         data dictionaries, metadata, source notes
├── scripts/                   Python or R code, numbered in the order they run
├── qgis/                      QGIS project files (.qgz) and saved styles
├── outputs/
│   ├── maps/                  exported map images and PDFs
│   └── tables/                summary tables and statistics
└── docs/                      reports, presentations and the thesis or paper
```

The details can vary, but three principles matter most:

1. **Raw data is read-only.** Keep the original download or field export exactly as you received it, and never
   edit it. Every change produces a new file in `processed/`. If something goes wrong, you can always start
   again from the original. (On most systems you can mark the folder or files as read-only.)
2. **Separate inputs, processing and outputs.** Anyone should be able to tell what came from elsewhere, what you
   made, and what you are presenting.
3. **Document the structure** in a README at the top of the project.

## File naming

Good file names are **descriptive**, **consistent** and **machine-friendly**.

| Instead of | Use | Why |
|---|---|---|
| `final_FINAL_v2 (copy).shp` | `tippecanoe_tracts_2024_utm16n.gpkg` | Says what, where, when and in which CRS |
| `Tracts New.gpkg` | `tracts_new.gpkg` | No spaces or capitals; works in every program and in scripts |
| `map 3-4-26.png` | `2026-03-04_no_car_households_map.png` | ISO 8601 dates (YYYY-MM-DD) sort in date order |
| `data.csv` | `acs2024_tippecanoe_tracts_b08201.csv` | Unique and self-explanatory even outside its folder |
| `clean.py`, `analysis.py` | `01_download.py`, `02_clean.py`, `03_analysis.py` | Numbers show the order to run them |

Rules of thumb:

- Use only letters, numbers, underscores (`_`) and hyphens (`-`). Avoid spaces and characters such as
  `& % # ( ) ' /`.
- Put the most important element first, so related files sort together.
- Write dates as `YYYY-MM-DD` (or `YYYYMMDD`).
- Pad numbers with zeros (`01`, `02`, ... `10`) so they sort correctly.
- Agree on the convention with your team and write it in the README.

## Keep QGIS projects portable

A QGIS project file (`.qgz`) does not contain the data; it stores **paths** to each layer. If those paths are
**absolute** (`C:\Users\Maria\Desktop\project\data\roads.gpkg`), the project breaks as soon as the folder is
moved, renamed or opened on another computer.

- Use **relative paths** (`../data/processed/roads.gpkg`). In QGIS this is the default, set in **Project >
  Properties > General > Save paths**.
- Keep data **inside** the project folder rather than scattered across the computer.
- If layers break, QGIS opens a **Handle Unavailable Layers** dialog where you can point each layer to its new
  location.

## Version control

Data and code change constantly during a project. Without a system, you end up with a folder of near-identical
files and no idea which one is current.

### Simple versioning

For small projects, a clear manual system works:

- Add a version number or date to processed files: `stores_v01.gpkg`, `stores_v02.gpkg`, or
  `stores_2026-03-04.gpkg`.
- Keep a **change log** (a plain text file) that records what changed in each version and why:

```
2026-03-04  stores_v02.gpkg  Removed 2 duplicate stores (Fresh Foods, Main St.); fixed 5 store_type values.
2026-02-20  stores_v01.gpkg  First cleaned version of the field export.
```

- Never reuse a version number, and never edit an old version.

### Git and GitHub

**Git** is a version control system that records every change to a set of files, who made it and why, and
lets you return to any earlier state. **GitHub** hosts Git repositories online for sharing and collaboration
(these lessons are themselves Git repositories on GitHub).

Git is excellent for **code, documentation and small text-based data** such as CSV and GeoJSON files, where it
can show exactly which lines changed. It is a poor fit for large binary files (imagery, large GeoPackages):
it cannot show what changed inside them, and repositories grow quickly. GitHub warns about files larger than
50 MB and blocks files larger than 100 MB. Large data is better kept in research storage or a data repository,
with only the code and documentation in Git.

## Storage and backup

### Where to keep the working copy

| Option | Good for | Watch out for |
|---|---|---|
| Your laptop | Fast day-to-day work | Theft, loss, hardware failure |
| External drive | A local backup copy | Easy to lose; drives fail; keep it in a different place |
| Institutional cloud or research storage | Collaboration and secure, backed-up storage | Check what kinds of data your institution allows there |
| Commercial cloud storage | Convenient syncing | Data protection rules; syncing also spreads accidental deletions |

Universities provide approved storage for research data; at Purdue, Research Computing and the Libraries can
advise on the right option for a project. Sensitive data (for example, data about identifiable people) may only
be stored in approved, secure locations.

::::::::::::::::::::::::::::::::::::: callout

## Syncing is not backup

A synced folder copies your mistakes too: delete or overwrite a file and the change reaches every copy within
seconds. A **backup** is a separate copy, ideally with older versions kept, that you can restore from.

::::::::::::::::::::::::::::::::::::::::::::::::

### The 3-2-1 rule

Keep at least:

- **3** copies of your data (the working copy plus two backups),
- on **2** different types of storage (for example, your laptop and a cloud service),
- with **1** copy off-site (in a different building, or in the cloud).

And **test** your backups: a backup you have never restored from is a hope, not a plan.

::::::::::::::::::::::::::::::::::::: challenge

## Rename these files

Rewrite each name following the conventions in this episode:

1. `Final Map (2).png`, a map of grocery stores made on 7 March 2026
2. `tracts.shp`, Indiana census tracts for 2024 in UTM zone 16N
3. `New Microsoft Excel Worksheet.xlsx`, ACS vehicle data for Tippecanoe County tracts
4. `script.py`, the second script in the workflow, which joins ACS data to tracts

:::::::::::::::: solution

1. `2026-03-07_grocery_stores_map.png`
2. `indiana_tracts_2024_utm16n.gpkg` (also converting it to a GeoPackage)
3. `acs2024_tippecanoe_tracts_vehicles.csv` (as CSV for a durable, open copy)
4. `02_join_acs_to_tracts.py`

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Is this backed up?

A researcher keeps her project on her laptop, in a folder synced to a cloud service. Does she meet the 3-2-1
rule? What could still go wrong?

:::::::::::::::: solution

Not quite. She has two copies on two kinds of storage, one of them off-site, but syncing means an accidental
deletion, overwrite or ransomware infection on the laptop is copied to the cloud. She needs a third, separate
copy, for example a scheduled backup to an external drive or a backup service that keeps older versions.
Checking that the cloud service keeps version history (and for how long) would also help.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Use a consistent folder structure that separates raw, processed and output data, and keep raw data read-only
- Name files descriptively and consistently, without spaces, with ISO 8601 dates and zero-padded numbers
- Keep data inside the project folder and use relative paths so QGIS projects stay portable
- Track versions with numbered files and a change log, or with Git for code and small text data
- Follow the 3-2-1 backup rule, remember that syncing is not backup, and test your backups

::::::::::::::::::::::::::::::::::::::::::::::::
