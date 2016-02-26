Unigraph Public Schema
======================

The Unigraph Schema is the backbone of the UniGraph knowledge graph. It is inspired by the freebase schema with major modifications in the following areas:

Measured dimensions
-------------------

Measured dimensions are everywhere: people height, mountain elevation, engine power etc. We have rewritten all measured dimensions from scratch and we have made huge logical improvements compared to freebase.

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

Measurement units now include all the conversion information between units

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
        {
            "Id": "measured_dimension.distance_unit.si_base_conversion_formula",
            "Name": "Convertion formula",
            "Description": "Convertion formula to the System International base unit",
            "ObjectType": "type.rawstring",
            "Unique": true,
            "Required": false
        }
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

You are passionate about a subject? Please feel free to fork, edit, improve it. All pull requests are highly appreciated. You can always drop us a line on the contacts listed on [http://unigraph.rocks](http://unigraph.rocks)
