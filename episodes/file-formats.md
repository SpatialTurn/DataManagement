---
title: "Geospatial File Formats"
teaching: 30
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

- What are the common file formats for vector and raster data?
- What are the limitations of the shapefile?
- Which format should I use to work with, to share, and to archive my data?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Recognize the most common vector and raster file formats and their file extensions
- Explain the strengths and limitations of shapefiles, GeoPackages and GeoJSON
- Distinguish open from proprietary formats
- Choose a suitable format for working, sharing and long-term storage

::::::::::::::::::::::::::::::::::::::::::::::::

## Why formats matter

The same map layer can be stored in many file formats. The choice affects whether others can open your data,
whether attribute names and values survive, how large the files are, and whether the data will still be
readable in twenty years. A few questions to ask of any format:

- Is it **open** (a published, freely implementable specification) or **proprietary** (controlled by one
  company)?
- Is it **one file** or **several** that must stay together?
- Does it preserve the **coordinate reference system**, **field names**, **data types** and **null values**?
- How large can it get, and how fast is it?

## Vector formats

| Format | Extension(s) | Open? | Notes |
|---|---|---|---|
| **Shapefile** | `.shp` `.shx` `.dbf` `.prj` (and others) | Published by Esri | The old standard; widely supported but limited (see below) |
| **GeoPackage** | `.gpkg` | Yes (OGC standard) | One file holding many vector layers, tables and even rasters; the default in QGIS |
| **GeoJSON** | `.geojson` | Yes (IETF RFC 7946) | Plain text; ideal for the web and small datasets; always in longitude and latitude (WGS 84) |
| **KML / KMZ** | `.kml` `.kmz` | Yes (OGC standard) | Google Earth's format; good for sharing with the public, weak for analysis |
| **CSV with coordinates** | `.csv` | Yes | A table with x and y (or longitude and latitude) columns; only points; the CRS must be documented separately |
| **File geodatabase** | `.gdb` (a folder) | No (Esri) | ArcGIS's main format; readable by QGIS and GDAL, but best kept for ArcGIS workflows |
| **GeoParquet** | `.parquet` | Yes (open specification, version 1.1) | Columnar format for large datasets and cloud analysis; growing quickly |

### The trouble with shapefiles

The shapefile dates from the early 1990s. It is everywhere, and every GIS can read it, but it has real
limitations:

- **Several files make one dataset.** At minimum `.shp` (geometry), `.shx` (index) and `.dbf` (attributes);
  usually also `.prj` (coordinate system) and `.cpg` (text encoding). Lose the `.prj` and the layer no longer
  knows where it is; lose the `.dbf` and the attributes are gone.
- **Field names are limited to 10 characters.** `produce_available` becomes `produce_av`, and two long names
  can collide.
- **Text fields hold at most 254 characters**, and there is no true date-and-time type.
- **Each file is limited to 2 GB.**
- **One geometry type per file**, and no way to store several layers together.
- **Null values are poorly supported**, so missing numbers often turn into 0.

None of these are a problem for a quick download, but they cause silent data loss when you save your own
carefully designed data as a shapefile.

::::::::::::::::::::::::::::::::::::: callout

## Prefer GeoPackage for your own work

A GeoPackage is a single file (an SQLite database) that can hold all the layers of a project, with long field
names, real date and time fields, proper null values and no 2 GB limit. It is an Open Geospatial Consortium
(OGC) standard and the default format in QGIS. If you receive shapefiles, it is good practice to convert them
to a GeoPackage at the start of a project.

::::::::::::::::::::::::::::::::::::::::::::::::

## Raster formats

| Format | Extension(s) | Notes |
|---|---|---|
| **GeoTIFF** | `.tif` `.tiff` | The standard raster format: a TIFF image with georeferencing built in; supports many bands and data types |
| **Cloud Optimized GeoTIFF (COG)** | `.tif` | A GeoTIFF organized so programs can read just the part they need over the web; an OGC standard |
| **JPEG 2000** | `.jp2` | Compressed imagery; used by Sentinel-2 |
| **NetCDF** and **HDF5** | `.nc` `.h5` | Multidimensional arrays (for example, temperature by latitude, longitude, height and time); common for climate and Earth observation data |
| **Zarr** | a folder | A cloud-friendly format for very large multidimensional arrays |
| **ASCII grid** | `.asc` | A plain-text grid; easy to read, but large and slow |
| **PNG or JPEG with a world file** | `.png` + `.pgw` | An ordinary image plus a small text file of georeferencing; fine for display, not for analysis |

### Compression

Raster files can be **compressed** to save space. **Lossless** compression (such as DEFLATE or LZW in a
GeoTIFF) keeps every value exactly. **Lossy** compression (such as JPEG) discards detail to save more space,
which is acceptable for a background image but not for data you will measure or analyze.

## Tables

Tables without geometry are often the largest part of a social science project. **CSV** is the safest
format for sharing them: it is plain text, opens anywhere, and will remain readable for decades. Spreadsheet
files (`.xlsx`) are convenient for editing but can hide problems: merged cells, formulas, colors that carry
meaning, and automatic conversion of codes such as `01001` into the number `1001` or of `MAR1` into a date.

::::::::::::::::::::::::::::::::::::: callout

## Watch out for leading zeros

FIPS codes and GEOIDs often start with zero (`01` is Alabama, `01001` is Autauga County). Opening a CSV in a
spreadsheet program can silently turn them into numbers and drop the zero, breaking every later join. Import
ID columns as **text**, and check them after every conversion.

::::::::::::::::::::::::::::::::::::::::::::::::

## Which format when?

| Purpose | Good choices | Why |
|---|---|---|
| **Working copy** of a project | GeoPackage (vector), GeoTIFF (raster) | Complete, fast, few limitations |
| **Sharing** with collaborators | GeoPackage, or the format they use | One file is easy to send |
| **Web maps and small datasets** | GeoJSON | Readable by web libraries and by people |
| **Very large vector data** | GeoParquet | Fast, compact, cloud-friendly |
| **Large imagery shared online** | Cloud Optimized GeoTIFF | Readers can fetch only what they need |
| **Long-term preservation** | Open, well-documented formats: GeoPackage, GeoJSON, GeoTIFF, CSV | Readable without any one company's software |

## Converting between formats

Conversions are routine (in QGIS, right-click a layer and choose **Export > Save Features As...**), but each
one is a chance to lose information. After converting, check:

- that the **coordinate reference system** is still correct
- that **field names** were not shortened and **data types** did not change (text IDs becoming numbers)
- that **null values** did not become 0 or empty strings
- that the **number of features** is the same
- that **special characters** (accents, non-Latin scripts) display correctly

Behind most GIS software, including QGIS, is the open-source **GDAL** library, which reads and writes more
than 200 formats. That is why QGIS can open almost anything you are given.

::::::::::::::::::::::::::::::::::::: challenge

## What went wrong?

A colleague sends you a zip file containing `stores.shp` and `stores.dbf`. When you open it in QGIS, the
points appear near the coast of Africa, and a field called `produce_av` holds values that should be in
`produce_available`. What happened, and what would you ask for?

:::::::::::::::: solution

The `.prj` file is missing, so QGIS does not know the layer's coordinate reference system. The points are
probably in a projected system (with coordinates in meters) being read as degrees, or in degrees being read as
the wrong system, so they land near 0° latitude, 0° longitude. The `.shx` index is also missing, which some
software cannot do without. The shortened field name is the shapefile's 10-character limit. Ask for the
complete set of files (including `.shx` and `.prj`), or better, a GeoPackage, plus the CRS (for example its
EPSG code).

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Choose a format

What format would you use for each, and why?

1. A field team's grocery store points, with long field names and a date and time for each visit
2. A small layer of 20 campus buildings for an interactive web map
3. A 40 GB set of Landsat images to share with collaborators who will analyze small areas
4. A table of ACS estimates by census tract that will be archived with a thesis

:::::::::::::::: solution

1. **GeoPackage**: long field names, true date and time fields and proper nulls; a shapefile would truncate
   names and lose the times.
2. **GeoJSON**: small, plain text, and read directly by web mapping libraries.
3. **Cloud Optimized GeoTIFF**: collaborators can read only the areas they need instead of downloading 40 GB.
4. **CSV**, with GEOIDs stored as text and a data dictionary describing each column: open, simple and durable.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Formats differ in openness, number of files, size limits and what attribute information they preserve
- Shapefiles are universal but limited: several files, 10-character field names, 2 GB, weak null support
- GeoPackage is a good default for your own vector work; GeoTIFF for rasters; CSV for tables
- Prefer open, well-documented formats for sharing and preservation
- Check the CRS, field names, data types, nulls and feature counts after every conversion

::::::::::::::::::::::::::::::::::::::::::::::::
