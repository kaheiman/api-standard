# Naming Conventions

## General Naming Rules

+ Use American English
+ Don't use acronyms
+ Use _lowerCamelCase_ unless stated otherwise (as in URIs, which use kebab-case).
+ Collection resources __MUST__ have plural names, unless a plural is not available (like _furniture_).
+ Enumerations should follow _SCREAMING_SNAKE_CASE_

Every identifier **MUST** be in American English and written in lowercase. An identifier **SHOULD NOT** contain acronyms. CamelCase (camelCase) **MUST** be used to delimit combined words.

## URI

Every URI **MUST** follow the General Rules except for the camelCase rule. Instead, a hyphen (-) **SHOULD** be used to delimit combined words (kebab-case). Besides, a URI **MUST NOT** end with a trailing slash (/).

### Example

A well-formed URI:

> `/system-orders/1234/author`

## Resource Identifiers

Resource identifiers are unique strings that identify a particular resource. A resource identifier **SHOULD** be a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier), **MUST** be a unique string, and **MUST NOT** be a [URN](URNs.md).

### Example

`656f4ea3-a6fb-4071-9fb9-404ad9e7ca9c`

## Query Parameters and Path Fragments

Every URI query parameter or fragment **MUST** follow the General Rules. Also, they **MUST NOT** clash with the reserved query parameter names.

## URI Template Variables

In addition to General Naming Rules, URI Template Variable names **MUST** follow the RFC6570. That is, the variable names can consist only from ALPHA / DIGIT / "_" / pct-encoded.

> **NOTE:** Per RFC6570 Hyphen (-) is NOT legal URI Template variable name character.

### Example

A well-formed URI Template Variable:

`./system-orders/{orderId}/author`

## Representation Format Fields

Every representation format field **MUST** conform to the General Naming Rules.

### Example

A well-formed resource representation:

    {
      "orderNumber": 1234,
      "itemCount": 42,
      "status": "PENDING"
    }

## Representation of ENUMS

Enums represent a small fixed set of values that typically do not change.
Every enum should follow `SCREAMING_SNAKE_CASE` convention.

### Example
  
  {
    "gender": "MALE"
    "status": "PENDING_ACTIVATION",
  }

## HTTP Headers

Custom headers names __MUST__ use `kebab-case`.

A custom HTTP Header __SHOULD NOT__ start with `X-` (RFC6648).

Custom headers created by Autodesk __MUST__ start with the `adsk-` prefix. To provide additional scoping, custom headers __MAY__ add service information in the form `adsk-<service>` as the header prefix.

Custom headers __MUST NOT__ contain underscore (_) characters.

### Examples

    adsk-order-metadata: 42
