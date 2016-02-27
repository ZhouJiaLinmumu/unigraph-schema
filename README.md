The Unigraph Schema
======================

The Unigraph Schema is the backbone of the UniGraph Knowledge Graph. It is inspired by the freebase schema with major modifications in the areas described below. You can use the schema to map the objects and their relations from the world we live in. For example, the data from the London DataStore about the  [Birth and Death rates by Ward](http://data.london.gov.uk/dataset/birth-and-death-rates-ward/resource/97375da0-d625-4e2f-aa35-a77d106b3d85) can be represented like this:

```
Ward > dataworld.sameas.uk_ons_gss_code
Births 2002 > measured_dimension.dated_float.value
Ward Name > type.object.name
Ward Name > location.statistical_region.births > measured_dimension.dated_float
Ward Name > location.location
Borough > type.object.name
Borough > location.location
Borough > location.location.contains > Ward Name
```

Measured dimensions
-------------------

Measured dimensions are everywhere: people height, mountain elevation, engine power etc. We've created all measured dimensions from scratch and combined them in a single domain together with their respective measurement_unit.

```
{
    "Id": "measured_dimension.distance",
    "Name": "Distance",
    "Description": "",
    "Mediator": true,
    "ExpectedTypes": null,
    "Enumerated": false,
    "Properties": [
        {
            "Id": "measured_dimension.distance.measurement_unit",
            "Name": "Unit of Distance",
            "Description": "",
            "ObjectType": "measured_dimension.distance_unit",
            "Unique": true,
            "Required": true
        },
        {
            "Id": "measured_dimension.distance.value",
            "Name": "Value",
            "Description": "",
            "ObjectType": "type.float",
            "Unique": true,
            "Required": true
        }
    ]
}
```

Measurement Units
-----------------

Measurement units include all necessary conversion information to the International System of Units (SI).

```
{
    "Id": "measured_dimension.distance_unit",
    "Name": "Unit of Length",
    "Description": "A Unit of length is any measure used for linear distance (height, width, etc.). If the unit is fixed, such as the American inch, its length in the SI base unit, meters, should be given. If it is variable or unknown, such as the Biblical cubit, then the meter equivalence should not be specified.",
    "Mediator": false,
    "ExpectedTypes": [
        "common.topic"
    ],
    "Enumerated": true,
    "Properties": [
        {
            "Id": "measured_dimension.distance_unit.abbreviations",
            "Name": "Abbreviations",
            "Description": "Abbreviated text representations for this unit",
            "ObjectType": "type.rawstring",
            "Unique": false,
            "Required": false
        },
        {
            "Id": "measured_dimension.distance_unit.canonical_abbreviation",
            "Name": "Canonical abbreviation",
            "Description": "Globally recognised abbreviated text representation for this unit.",
            "ObjectType": "type.rawstring",
            "Unique": true,
            "Required": false
        },
        {
            "Id": "measured_dimension.distance_unit.measurement_system",
            "Name": "Measurement System",
            "Description": "The measurement system this unit belongs to",
            "ObjectType": "measured_dimension.measurement_system",
            "Unique": false,
            "Required": false
        },
        {
            "Id": "measured_dimension.distance_unit.dimension",
            "Name": "Measured dimension",
            "Description": "The dimension measured by this unit",
            "ObjectType": "measured_dimension.dimension",
            "Unique": true,
            "Required": false
        },
        **{
            "Id": "measured_dimension.distance_unit.si_base_conversion_formula",
            "Name": "Convertion formula",
            "Description": "Convertion formula to the System International base unit",
            "ObjectType": "type.rawstring",
            "Unique": true,
            "Required": false
        }**
    ]
}
```

Self contained domains
----------------------

Domains no longer leak information. Cross-domain references are handled by type inheritance. In the below example the "book.book_edition_location" type will inherit all properties from its expected type: "location.location", while in the same time the particular location that the book was published will also be referenced as such and not just as "location.location":

```
{
    "Id": "book.book_edition_location",
    "Name": "Book Edition Location",
    "Description": "The place, usually a city, where a book edition has been published.",
    "Mediator": false,
    "ExpectedTypes": [
        "location.location",
        "common.topic"
    ],
    "Enumerated": true,
    "Properties": null
}
```

SameAs
------

In the domain Dataworld, we have created a type `dataworld.sameas` which contains more than 800 properties linking data from external repositories of information to UniGraph. Many include examples.

```
        {
            "Id": "dataworld.sameas.uk_companies_house_id",
            "Name": "UK Companies House Company ID",
            "Description": "The assigned number of the company by the UK Companies House. Examples: 8209948, SC421617, FC031362, IP10067R",
            "ObjectType": "type.rawstring",
            "Unique": true,
            "Required": false
        }
```

Contributions are welcome
-------------------------

Passionate about a subject? Feel free to fork, edit and improve the schema. All pull requests are highly appreciated. You can always drop us a line on the contacts listed at [unigraph.rocks](http://unigraph.rocks) and follow us on [twitter](http://twitter.com/unigraphrocks).

*Build with love in Slovakia. With the support of [ODINE](http://opendataincubator.eu)*
