---
title: "Designing Your Data"
teaching: 30
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I decide what layers and attributes a project needs?
- What is a data dictionary, and what goes in it?
- How do field types, domains and unique IDs keep data consistent?
- How do tables relate to each other and to map layers?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Translate a research question into layers, geometries and attributes
- Write a data dictionary with field names, types, lengths, domains and null rules
- Explain the role of primary keys and foreign keys in joining data
- Recognize one-to-one, one-to-many and many-to-many relationships

::::::::::::::::::::::::::::::::::::::::::::::::

## Start from the question

Designing a dataset (sometimes called **schema design** or, in a GIS, **geodatabase design**) is the most
important and most time-consuming part of building one. Decisions made here are hard to change later, after
hundreds of records have been entered. Work through these steps on paper or in a spreadsheet before creating
anything in a GIS.

### Step 1: What things will you record?

List each kind of object or phenomenon you will record. Each usually becomes its own **layer** (or table).
For a study of food access, these might be grocery stores, bus stops, and survey respondents' neighborhoods.

### Step 2: What geometry suits each one?

Decide whether each is best represented as a point, line or polygon at the scale you will analyze it (see the
**Geospatial Data Models** episode). A store is a point for an access study but could be a polygon for a study
of store size.

### Step 3: What do you need to know about each one?

Brainstorm the **attributes**: the questions you will ask of the data. For grocery stores:

| Type of information | Example attributes |
|---|---|
| Identity of the store | store ID, name, chain or brand |
| Location | address, city, ZIP code, coordinates |
| Benefits accepted | SNAP accepted, WIC accepted |
| Food available | fresh produce available, fresh meat available |
| When it was observed | date and time of the visit, hours posted |
| Anything else relevant | notes |
| Evidence | storefront photograph |

Only collect what you will use. Every extra field costs time to collect and check, and personal information
creates obligations to protect it.

## The data dictionary

A **data dictionary** turns that list into precise **fields**. It is both a design tool and a key piece of
documentation: it tells everyone who enters or uses the data exactly what each field means.

Table: Data dictionary for the grocery store point layer

| Field name | Description | Type | Length | Allowed values (domain) | Nulls allowed? |
|---|---|---|---|---|---|
| `store_id` | Unique ID for each store | Integer | | Assigned automatically, never reused | No |
| `store_name` | Name shown on the store's sign | Text | 60 | Free text | No |
| `store_type` | Kind of store | Text | 20 | Supermarket, Small market, Convenience, Other | No |
| `address` | Street address | Text | 80 | Free text | Yes |
| `city` | City | Text | 30 | Lafayette, West Lafayette, Other | No |
| `zip` | ZIP code | Text | 5 | Five digits | Yes |
| `snap_accepted` | SNAP benefits accepted, based on posted signs | Text | 7 | Yes, No, Unknown | No |
| `wic_accepted` | WIC benefits accepted, based on posted signs | Text | 7 | Yes, No, Unknown | No |
| `produce_available` | Fresh produce on sale at the visit | Text | 7 | Yes, No, Unknown | No |
| `visit_datetime` | Date and time of the field visit | Date and time | | | No |
| `hours_posted` | Opening hours as posted on the door | Text | 100 | Free text | Yes |
| `notes` | Anything unusual about the visit | Text | 250 | Free text | Yes |
| `photo` | Storefront photograph | Attachment (file path) | | | Yes |

### Field types

Choose the type that matches how the value will be used:

| Type | Use for | Watch out for |
|---|---|---|
| **Integer** | Counts and ID numbers you will calculate with | Not for codes with leading zeros |
| **Decimal** (real, double) | Measurements: distances, percentages, prices | Set enough precision |
| **Text** (string) | Names, categories, and codes such as ZIP codes and GEOIDs | Set a sensible maximum length |
| **Date** and **date and time** | When something happened | Shapefiles cannot store times; use GeoPackage |
| **Boolean** (true or false) | Simple yes or no | Cannot say "unknown"; a Yes, No, Unknown text domain is often better |

A useful rule: **if you would never do arithmetic with it, store it as text.** ZIP codes, FIPS codes, GEOIDs
and phone numbers are labels, not quantities.

### Domains

A **domain** is the list (or range) of values a field may hold. Dropdown menus in a field app enforce it.
Domains prevent the most common data entry problem: the same thing written many ways ("SNAP", "snap", "EBT",
"yes"). In QGIS, domains are set in **Layer Properties > Attributes Form** with the **Value Map** widget; in a
GeoPackage or file geodatabase they can also be stored in the data itself.

Include **Unknown** in categorical domains. A blank can mean "unknown", "not applicable" or "forgot to fill in";
an explicit Unknown is honest and unambiguous.

### Field names

Good field names are:

- **short but meaningful**: `snap_accepted`, not `snap` or `does_the_store_accept_snap_benefits`
- **lowercase with underscores**, no spaces or special characters, and not starting with a number
- **consistent**: if one field is `wic_accepted`, the next is `snap_accepted`, not `SNAPaccept`
- **10 characters or fewer** only if the data must ever be saved as a shapefile

## Unique IDs and keys

Every feature needs a **unique identifier** that never changes and is never reused. Names make poor IDs: two
stores can share a name, and names change. Coordinates make poor IDs too, because they change when a point is
corrected.

In database terms:

- A **primary key** uniquely identifies each row of a table (`store_id` in the store layer; `GEOID` in a census
  tract layer).
- A **foreign key** is a field in another table that refers to a primary key (a `store_id` column in a table of
  repeated visits; a `GEOID` column in a table of ACS estimates).

Joins work by matching keys, so keys must have **identical values and types** in both tables. A GEOID stored
as text in one table and as a number in the other will not match.

## Relationships between tables

| Relationship | Example | How to handle it |
|---|---|---|
| **One-to-one** | Each census tract has one row of ACS estimates | A simple **join** adds the columns to the tract layer |
| **One-to-many** | Each store is visited several times | Keep visits in a **separate table** with a `store_id` foreign key, and link it with a **relation** rather than copying store details into every visit |
| **Many-to-many** | Stores accept many benefit programs; each program is accepted by many stores | Use a third **linking table** with one row per store and program |

Keeping repeated information in separate, linked tables (called **normalization**) avoids inconsistencies: if a
store's name is stored once, it cannot be spelled two different ways. QGIS supports this through **Project
Properties > Relations**, and a GeoPackage can hold all the linked tables in one file.

::::::::::::::::::::::::::::::::::::: callout

## Some choices are easy to change later, some are not

Adding a new field later is easy. Renaming fields, changing a field's type, changing the geometry type, or
changing the meaning of a category after data has been collected is hard and error-prone. Spend the time on the
design, test it on a few real records, and only then start collecting.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Fix the data dictionary

What is wrong with each of these field definitions, and how would you fix it?

| Field name | Type | Allowed values |
|---|---|---|
| `Store Name` | Text | |
| `zip` | Integer | |
| `open` | Boolean | true, false |
| `type` | Text | free text |

:::::::::::::::: solution

- `Store Name` contains a space and capitals; use `store_name`.
- `zip` as an integer drops leading zeros (`02134` becomes `2134`) and invites meaningless arithmetic; store it
  as text with a length of 5.
- `open` is ambiguous (open now? open 24 hours?) and a Boolean cannot say "unknown". Use a clearer name, such as
  `open_at_visit`, with a Yes, No, Unknown domain.
- `type` as free text will produce inconsistent categories; give it a domain such as Supermarket, Small market,
  Convenience, Other. A more specific name (`store_type`) also helps.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Design your own

Choose a topic from your own research or teaching (for example, park amenities, public art, or vacant lots).
Write a data dictionary with at least six fields, including a unique ID, at least two fields with domains, and
a date field. Decide which fields may be null.

:::::::::::::::: solution

Answers will vary. Check your design against these questions:

- Does every feature get a unique, permanent ID?
- Do categorical fields have domains that include Unknown?
- Are codes stored as text and measurements as numbers?
- Are field names short, lowercase and free of spaces?
- Is there a date (and time, if it matters) for when each record was observed?
- Does every field have a description someone else could understand?

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Design data from the research question: the things you record become layers, and the questions become attributes
- A data dictionary defines each field's name, meaning, type, length, allowed values and null rule
- Store codes such as ZIP codes and GEOIDs as text; use domains with an Unknown option for categories
- Every feature needs a permanent unique ID; joins match primary keys to foreign keys
- Keep repeated information in separate related tables rather than copying it

::::::::::::::::::::::::::::::::::::::::::::::::
