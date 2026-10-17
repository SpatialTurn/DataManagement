---
title: 'Instructor Notes'
---

## Overall approach

- This module is conceptual. Each episode takes about 30 minutes plus 15 to 20 minutes for challenges, so the
  whole module fits in a half day to a full day, or can be split across sessions.
- The examples follow one project throughout: mapping grocery stores and food access in Lafayette and West
  Lafayette, Indiana. The same example runs through the Data Collection and Case Studies modules, so learners
  can see management decisions applied to real data.
- Encourage learners to bring a project of their own (a thesis, a class project) and apply each episode to it.

## Episode by episode

### Introduction to Data Management

- Start with the "Spot the risks" challenge: most learners recognize themselves in it.
- If learners are applying for grants, show a real data management plan template in DMPTool.

### Geospatial Data Models

- The scale examples (a city as a point or a polygon) prompt good discussion; ask for examples from learners'
  own work.
- The resolution challenge makes the cost of fine rasters concrete.

### Geospatial File Formats

- Show a shapefile's files in a file browser, then the same data as one GeoPackage. Then show a field name being
  shortened to 10 characters on export.
- The leading-zero problem with FIPS codes in spreadsheets is the most common real-world error; demonstrate it if
  you can.

### Coordinate Reference Systems

- The "define versus reproject" distinction is the key idea. A live demonstration of assigning the wrong CRS (and
  watching a layer jump) is memorable.
- Learners from the Data Collection module have already seen network costs reported in degrees when the project
  CRS was wrong.

### Designing Your Data

- Have learners draft a data dictionary in pairs, then swap and critique.

### Organizing, Versioning and Backing Up

- Ask learners how they back up their data now. Many rely on synced folders alone.

### Data Quality

- The figure of topology errors works well as a "spot the error" exercise before revealing the labels.

## Common questions

| Question | Short answer |
|---|---|
| "Should I stop using shapefiles?" | Use them to exchange data when needed, but keep your own working data in GeoPackage |
| "Which CRS should I use?" | Store data in its original CRS; analyze in a projected CRS suited to the area (in Indiana, UTM zone 16N is a good default) |
| "Is GitHub enough for backup?" | It is good for code and small text data, but not for large data, and it is not a backup strategy on its own |
| "Do I need a DMP for my thesis?" | Usually not formally, but writing one takes an hour and prevents many problems |
