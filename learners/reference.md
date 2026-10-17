---
title: 'Reference'
---

## Glossary

3-2-1 rule
:   A backup guideline: keep three copies of data, on two different types of storage, with one copy off-site.

Attribute
:   A piece of information about a feature, stored as a column in the attribute table.

Checksum
:   A short code calculated from a file's contents; if the file changes, the checksum changes.

Coordinate reference system (CRS)
:   The datum, coordinate system, units and (for projected systems) projection that link coordinates to places
    on Earth.

Data dictionary
:   A table describing every field of a dataset: name, meaning, type, length, allowed values and null rules.

Data management plan (DMP)
:   A document describing what data a project will create and how it will be handled, shared and preserved.
    NSF calls it a Data Management and Sharing Plan.

Data model
:   A way of representing the world as data; the two main geospatial data models are vector and raster.

Datum
:   A model of the Earth's shape and its position, such as WGS 84 or NAD83.

Domain
:   The set of values a field is allowed to hold, often presented as a dropdown list.

EPSG code
:   A numeric identifier for a coordinate reference system, such as EPSG:4326 or EPSG:26916.

Foreign key
:   A field in one table that refers to the primary key of another, used to join or relate them.

GeoPackage
:   An open, single-file format (an SQLite database) for vector layers, tables and rasters; an OGC standard.

Geographic coordinate system
:   A CRS that uses latitude and longitude in degrees.

GeoTIFF
:   A TIFF image with georeferencing information built in; the standard raster format.

Lineage
:   The record of where data came from and how it was processed; also called provenance.

NoData value
:   A special raster value meaning "no information" for a cell.

Primary key
:   A field that uniquely identifies each row of a table.

Projected coordinate system
:   A CRS that flattens the Earth onto a plane, with coordinates usually in meters or feet.

Raster
:   Data stored as a grid of equal cells, each with a value.

Relative path
:   A file location given relative to the project folder, so a project still works when the folder is moved.

Reprojecting
:   Calculating new coordinates for data in a different CRS (as opposed to defining which CRS existing
    coordinates are in).

Resolution
:   For raster data, the ground size of one cell.

Shapefile
:   An older, multi-file vector format (`.shp`, `.shx`, `.dbf`, `.prj` and others) with limits on field names,
    size and data types.

Topology
:   The spatial relationships between features, such as shared boundaries and connected lines.

Vector
:   Data stored as points, lines or polygons, each with attributes.

Version control
:   A system for recording changes to files over time, such as numbered versions with a change log, or Git.

## Further reading

- [NSF: Data Management and Sharing Plan](https://www.nsf.gov/funding/data-management-plan)
- [DMPTool: templates and guidance for data management plans](https://dmptool.org/)
- [EPSG.io: look up coordinate reference systems](https://epsg.io/)
- [GeoPackage](https://www.geopackage.org/)
- [QGIS User Guide](https://docs.qgis.org/latest/en/docs/user_manual/)
