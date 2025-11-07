---
title: Basics of Feature Class Manipulation
teaching: 10
exercises: 2
---

::::::::::::::::::::::::: questions

- What are the three basic Vector data geometries?
- How do you create a feature dataset in QGIS?
- How do you decide what datapoints should be included in a dataset?
:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::: objectives

- Understand the basic components of vector geometry
- Learn to create a data dictionary
- Learn the basics of data collection
- Learn to prepare a project file in QGIS
:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Beginner - Characteristics of geospatial data

When working with spatial data, you will come across two main spatial data formats: vector and raster. It is important to understand these components as we complete exercises and build geospatial concepts upon these components. 

### Vector Data Characteristics

Vector data is a coordinate based system that symbolizes features as three main geometries:

- point

- line

- polygon

![Points, Lines, and Polygons](fig\points_lines_polygons.jpg){alt="A comparison of the three main vector geometries: point, line, and polygon"}

Points can be imagined as **x** and **y** points on a graph that represent longitude and latitude respectively. 

Points often represent locations on a map that are too small to be displayed as polygons. 

::::::::::::::::::::::::: callout

Some examples of locations that may be represented as points include:

- Cell towers

- Weather stations

- Cities on a large scale map

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::



## Intermediate - Design and create a new feature dataset in QGIS

In this section, we outline the steps for creating a basic data collection workflow that can be accessed and used by students, collaborators, or public participants to collect field data on a desktop GIS or on a mobile device. The following steps assume that you have downloaded and installed a GIS software package (e.g., QGIS) and you have a new project open.  

### Design a layer to contain your incoming data

This is the most fundamental part of geodatabase design, and the most time consuming. This stage is where you will conceptualize what kind of vector data you need to collect to answer your research question, and how that translates into the spatial geometries of a GIS (e.g., points, lines, polygons, or all three). 

- First, consider what objects or phenomena field collectors will be recording to the map, that you will want in your database to later query, analyze and display for your project. Make a list of each kind of object or phenomenon you are interested in (e.g., variables in your study), since each may require a different data layer.

- Think about what shape/geometry those objects and data points will be most appropriately represented as at the scale you anticipate analyzing and displaying them. (TIP: a building might best be represented as a point on a small scale map, but as a polygon on a larger scale map).

- Next, brainstorm what kind of information (or, attributes) is important to collect about each feature in your layer. It can be helpful to use graph paper, spreadsheet software, whiteboard, etc., to create a table listing the kinds of information you want to collect, correlated with variables, measurements, or units, that can describe that information collectable as data. These will be driven by the variable your research is studying, so it is worth taking the time to map out these variables in a table format before creating your GIS layers. As an example, if you studying declining butterfly populations, the types of information that might inform your study could include information organized in the following table:

| Type of useful information                          | Example data points (attributes)                      |
|-----------------------------------------------------|--------------------------------------------------------|
| Traits about the butterfly                          | species, stage of development, condition              |
| What time of day was it spotted?                    | date and time                                         |
| What was the weather like?                          | temperature, cloud cover, precipitation               |
| Was the butterfly spotted on vegetation?            | plant species                                         |
| What was the location?                              | park name, address, coordinates                       |
| Should collectors provide a general description?    | description, comments field                           |
| Will a photograph be helpful?                       | field photo from phone                                |
                                
- Once you have brainstormed the categories of traits or attributes to collect about a feature or place, it’s time to translate these categories of data into spatial data “fields” that will form the attribute table for your GIS feature layer. This process can be referred to as building a “data dictionary”. Things to consider here: will each data point need a unique name or identifier code; will field collectors need to add measurements (if so, how precise); what kinds of observations will be entered in the collection form (numerical, textual, etc.); would pull-down menus be helpful to predefine selections available to collectors, should photos or attachments be added during data collection?

  - Some data properties can be changed later as your project evolves, but some are more difficult to change once they are established. So here again, visualizing your data dictionary in a spreadsheet is helpful, as in the example below:
  
Table: Data Dictionary Example Worksheet (point data)

| Attribute Description | Field Name | Data type | Length (text) | Data entry method    | Allow null values| Allow attachments |
|-----------------------|------------|-----------|---------------|----------------------|------------------|-------------------|
| Butterfly species     | Species    | String    | 40            | pull-down menu       | no               | yes               |
| Time of day           | Date_time  | Date      |               | Choose from calendar | no               | no                |
| Location of sighting  | Location   | String    | 40            | Enter text           | yes              | no                |
| Vegetation host       | Vegetation | string    | 20            | Enter text           | yes              |                   |

### Prepare a project file and editable data layers in QGIS

- Make sure you have downloaded and installed the [latest version of QGIS](https://www.qgis.org/download/)
- Launch QGIS and create a **new Project** using the blank page icon on the top right of the screen. Use the **Save as...** command in the Project tab to save your new project in a convenient location.
- To create a new feature layer, from the **Layer tab**, select **Create layer**, then select **New Shapefile Layer**. We can now configure the  new layer properties:
  1. In the **New Shapefile Layer** window, set the **File name** of the new shapefile in the file path to "Butterflies.shp"
  2. For Geometry, select **Point**.
  3. Use the default coordinate system, e.g., WGS84.
  4. In the **New Field** section, we will create attribute fields based on the spreadsheet you made earlier in the exercise.

:::::::::::::::::::: checklist

- Configure the data collection form (customize the data entry options, online/offline use, default entries, etc.)
- Your new feature layer is now ready to receive new point data and added to a new map for visualization.
- Repeat the steps above to create separate feature layers for the line features (e.g., trails or rivers) or polygon features (e.g., buildings, parks, or ponds).

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Advanced - Repeat the above steps, creating an original data dictionary based on your own research or teaching interests.



:::::::::::::::::::: keypoints

placeholder

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

