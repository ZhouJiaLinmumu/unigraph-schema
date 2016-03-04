# The Unigraph Schema

The Unigraph Schema is the backbone of the UniGraph Knowledge Graph. It is inspired by the freebase schema with major modifications in the areas described below.  
You can use the schema to map the objects and their relations from the world we live in. For example, the data from the London DataStore about the  [Birth and Death rates by Ward](http://data.london.gov.uk/dataset/birth-and-death-rates-ward/resource/97375da0-d625-4e2f-aa35-a77d106b3d85) can be represented like this:

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

## Key Concepts & Differentiators
[Measured dimensions](#dimensions)  
[Measured units](#units)  
[Self Contained Domains](#domains)
[SameAs](#sameas)  
[Strictly One to One connections](#one)  
[Unified Time Periods Representation](#periods) 

[Contributions](#contribute)

<a name="dimensions"/>
### Measured dimensions

Measured dimensions are everywhere: people height, mountain elevation, engine power etc. We've created all measured dimensions from scratch and combined them in a single domain together with their respective measurement_unit.

```json
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

<a name="units"/>
### Measurement Units

Measurement units include all necessary conversion information to the International System of Units (SI).

```json
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

<a name="domains"/>
### Self contained domains

Domains no longer leak information. Cross-domain references are handled by type inheritance. In the below example the "book.book_edition_location" type will inherit all properties from its expected type: "location.location", while in the same time the particular location that the book was published will also be referenced as such and not just as "location.location":

```json
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
This makes domains indipendant and keeps the schema clean.

<a name="sameas"/>
###SameAs

In the domain Dataworld, we have created a type `dataworld.sameas` which contains more than 800 properties linking data from external repositories of information to UniGraph. Many include examples.

```json
        {
            "Id": "dataworld.sameas.uk_companies_house_id",
            "Name": "UK Companies House Company ID",
            "Description": "The assigned number of the company by the UK Companies House. Examples: 8209948, SC421617, FC031362, IP10067R",
            "ObjectType": "type.rawstring",
            "Unique": true,
            "Required": false
        }
```

<a name="one"/>
### Strict one to one connections

We've done out best to keep all connections one to one. An example of this is the "book.author" type which has no properties of its own:

```json
{
    "Id": "book.author",
    "Name": "Author",
    "Description": "An author is a creator of a written or published work. The Author type is used for anyone who has written prose (whether fiction, essay, journalism, or scholarship), poetry, drama, or written or edited a book of any sort. This therefore includes editors of anthologies, whether or not the editor has written any material included in the anthology, and also includes artists in other media such as the fine arts and music, who may have had monographs or songbooks (for example) published. It also can include corporate authors, such as organizations, companies and government agencies, when a written work is credited to one, rather than to a person. It does not include scriptwriters for television and film (use TV Writer and Film Writer for these).",
    "Mediator": false,
    "ExpectedTypes": [
        "common.topic"
    ],
    "Enumerated": false,
    "Properties": null
}
```

It has many incoming properties though, for example from the "book.written_work" type:

```json
        {
            "Id": "book.written_work.author",
            "Name": "Author",
            "Description": "",
            "ObjectType": "book.author",
            "Unique": false,
            "Required": false
        }
```

<a name="periods"/>
### Unified periods representation

With small exceptions all references to a date that something started or ended follow the same pattern:

.start_date - denoting the beginning of the period  
.end_date - denoting the end of the period

```json
{
    "Id": "business.employment_tenure",
    "Name": "Employment tenure",
    "Description": "'Employment tenure' represents the relationship between a company and a person who has worked there. The company type typically tracks key employees, such as the management team, not all employees who have worked for a company.",
    "Mediator": true,
    "ExpectedTypes": null,
    "Enumerated": false,
    "Properties": [
...
        {
            "Id": "business.employment_tenure.start_date",
            "Name": "From",
            "Description": "",
            "ObjectType": "type.datetime",
            "Unique": true,
            "Required": false
        },
...
        {
            "Id": "business.employment_tenure.end_date",
            "Name": "To",
            "Description": "",
            "ObjectType": "type.datetime",
            "Unique": true,
            "Required": false
        }
    ]
}
```
The few exceptions are easy to predict:

```json
{
    "Id": "organization.organization",
    "Name": "Organization",
    "Description": "An organization is an organized body of members or people with a particular purpose; in the UniGraph context, a organization doesn't have a business association, like a company.  Companies have their own type in the business domain, located here. In addition, many types of organizations have their own, more specialized co-types that give them additional properties.",
    "Mediator": false,
    "ExpectedTypes": [
        "common.topic"
    ],
    "Enumerated": false,
    "Properties": [
...
        {
            "Id": "organization.organization.date_founded",
            "Name": "Date founded",
            "Description": "The date this organization first came into being.",
            "ObjectType": "type.datetime",
            "Unique": true,
            "Required": false
        }
```

<a name="contribute"/>
## Contributions are welcome

Passionate about a subject? Feel free to fork, edit and improve the schema. All pull requests are highly appreciated. You can always drop us a line on the contacts listed at [unigraph.rocks](http://unigraph.rocks) and follow us on [twitter](http://twitter.com/unigraphrocks).

*Build with love in Slovakia. With the support of [ODINE](http://opendataincubator.eu)*
