# Common Data Types

## Date and Time Format

Date and Time **MUST** always conform to the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format e.g.: *2017-06-21T14:07:17Z* (date time) or *2017-06-21* (date), it **MUST** use the UTC (without time offsets).

## Duration Format

Duration format **MUST** conform to the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard e.g.: *P3Y6M4DT12H30M5S* (three years, six months, four days, twelve hours, thirty minutes, and five seconds).

## Time Interval Format

Time intervals **MUST** be specified by two dates in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format, separated by ".." characters. e.g.:
*2007-03-01T13:00:00Z..2008-05-11T15:30:00Z*. Time intervals **MAY** also support other formats in the ISO 8601 format, which must be documented in the API.

## Id Format

A resource identifier **SHOULD** be in UUID format e.g.: *a3717085-b2b4-47a6-adfb-bf100be5d021*.

## Language Code Format

Language codes **MUST** conform to the [ISO 639](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) e.g.: *en* for English.

## Country Code Format

Country codes **MUST** conform to the [ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) alpha-2 e.g.: *DE* for Germany.

## Currency Format

Currency codes **MUST** conform to the [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) e.g.: *EUR* for Euro.

## Boolean Field

Boolean values **MUST** be represented as `true` or `false` boolean values and **MUST NOT** be strings.

## Optional Fields

* Optional Fields that are not set or are nulls **SHOULD NOT** be omitted from the response and **SHOULD** be output as `null` values.

## Regions

When an API specifies a region, it **MUST** follow this convention.

* Region names **SHOULD NOT** imply a specific location where data will be stored (country or city) or use of a specific cloud provider
* Region names **MAY** have one, two or three parts separated by “-“
  * The first part **SHOULD** be a market area: AMER, EMEA or APAC
  * The second part (if needed) **SHOULD** be a geographical direction: NORTH, SOUTH, EAST, WEST, CENTRAL, NE, NW, SE, SW, etc.
  * The third part (if needed) **SHOULD** be a single digit integer: 2, 3, 4, etc.
* Region names **SHOULD** use the minimum number of parts needed to distinguish them from other existing regions.
+ All services **MUST** use the same name for each new region.
  * Services in the existing US and EMEA regions which use different names for the regions (e.g. us-east, EU) SHOULD add support for the standard names
  * Services MAY also continue to support non-standard names to maintain backwards compatibility

Following this convention:

* US East (North Virginia) is US
* Dublin Ireland will continue to be EMEA
* Sydney will be APAC
* Japan will be APAC-NORTH
* Frankfurt will be EMEA-EAST
* Singapore will be APAC-CENTRAL

## Recommended Reading

* [The 5 laws of API dates and times](http://apiux.com/2013/03/20/5-laws-api-dates-and-times/)
* [Rationale for region naming convention](https://wiki.autodesk.com/pages/viewpage.action?pageId=852727019)
