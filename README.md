# STAC API - Context Extension Specification

- [STAC API - Context Extension Specification](#stac-api---context-extension-specification)
  - [Overview](#overview)
  - [ItemCollection fields](#itemcollection-fields)
  - [Context Object](#context-object)
  - [Example ItemCollection augmented with Context field](#example-itemcollection-augmented-with-context-field)

## Overview

- **Title**: Context
- **OpenAPI specification:** [openapi.yaml](openapi.yaml)
- **Conformance Classes:**
  - **STAC API - Item Search** binding: <https://api.stacspec.org/v1.0.0-rc.2/item-search#context>
  - **STAC API - Features** binding: <https://api.stacspec.org/v1.0.0-rc.2/ogcapi-features#context>
- **Scope:** STAC API - Features, STAC API - Item Search
- **[Extension Maturity Classification](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.2/README.md#maturity-classification):** Deprecated
- **Dependencies:**
  - [STAC API - Features](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.2/ogcapi-features)
  - [STAC API - Item Search](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.2/item-search)
- **Owner**: none

This extension is intended to augment the core [ItemCollection](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.2/itemcollection/README.md)
object when the ItemCollection is the result of a search, for example, from calling the `/search` API endpoint.

This fragment may be bound to either or both of 
[STAC API - Item Search](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.2/item-search) (`/search` endpoint) or
[STAC API - Features](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.2/ogcapi-features)
(`/collections/{collectionId}/items` endpoint) by
advertising the relevant conformance class. 

**Note**: OGC API Features - Part 1 has its own way returning `numberMatched` and `numberReturned` at the top level, instead of in a context
object. We are hoping to [align](https://github.com/opengeospatial/ogcapi-common/issues/82), but until then, it
is required to implement both when implementing STAC Features.

- [Example](examples/example.json)
- [JSON Schema](json-schema/schema.json)

## ItemCollection fields

| Element   | Type                              | Description                                                                                                                                                  |
| --------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `context` | [Context Object](#context-object) | **REQUIRED.** The search-related metadata for the [ItemCollection](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.2/itemcollection/README.md). |

## Context Object

| Element  | Type            | Description                                                                                                                           |
| -------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| returned | integer         | **REQUIRED** The count of results returned by this response. Equal to the cardinality of features array.                              |
| limit    | integer \| null | The maximum number of results to which the result was limited.                                                                        |
| matched  | integer         | The count of total number of results that match for this query, possibly estimated, particularly in the context of NoSQL data stores. |

The default sort of query results should be stable, but may not be depending on the data store's sorting performance.
It is recommended that the [Sort API Extension](../sort/README.md) be implemented in conjunction with this extension
and that fields conducive to stable sorting have sorting enabled over them.

**limit** - The maximum number of results requested explicitly, the default limit used by the service implementation
if no parameter was provided, or the maximum limit used by the service implementation if the limit parameter was larger.
`null` if no limit was placed on the query that retrieved these results, which should be a rare case in practice.

## Example ItemCollection augmented with Context field
  
```json
{
  "type": "FeatureCollection",
  "features": [ ],
  "context": {
    "returned": 9,
    "limit": 10, 
    "matched": 1092873
  }
}
```
