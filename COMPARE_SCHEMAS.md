# Endpoint: Compare Schemas (Path: /compareSchemas)
Compare two JSON Schemas regarding their relationship.
For a more general overview of the API please see the [start page of the API documentation](./YET_ANOTHER_WITNESS_API_DOC.md).

## Table of Contents
- [Endpoint: Compare Schemas (Path: /compareSchemas)](#endpoint-compare-schemas-path-compareschemas)
  - [Table of Contents](#table-of-contents)
  - [Request](#request)
    - [Request Headers](#request-headers)
    - [Request Body](#request-body)
      - [Parameters](#parameters)
        - [leftSchema](#leftschema)
        - [rightSchema](#rightschema)
  - [Response](#response)
    - [Response Headers](#response-headers)
    - [Response Body](#response-body)
      - [Parameters](#parameters-1)
        - [leftWitness](#leftwitness)
        - [rightWitness](#rightwitness)
        - [result](#result)
  - [Examples](#examples)
    - [Request](#request-1)
    - [Response](#response-1)

## Request
### Request Headers
Header Content-Type should be set to "application/json".
### Request Body
The request body generally conforms to the following structure:
```JSON
{
    "leftSchema": "<left JSON Schema>",
    "rightSchema": "<right JSON Schema>"
}
```
#### Parameters
##### leftSchema
- Type: String
- Description: stringified JSON Schema

##### rightSchema
- Type: String
- Description: stringified JSON Schema

## Response
### Response Headers
Content-Type will be set to "application/json" and HTTP status code will be set to either 200, 400, 404 or 500.
### Response Body
For a successful request (HTTP status code = 200) the expected result will look like this:
```JSON
{
    "timestamp": "<stringTimestamp>",
    "unixTimestamp": "<unixTimestamp>",
    "status": "<status>",
    "data": {
        "leftWitness": "<leftWitness>",
        "rightWiness": "<rightWitness>",
        "result": "<relationship>"
    }
}
```

#### Parameters
##### leftWitness
- Type: String
- Description: witness in its String representation, which conforms to the left but not to the right schema 

##### rightWitness
- Type: String
- Description: witness in its String representation, which conforms to the right but not to the left schema

##### result
- Type: String (String enum)
- Description: description of the relationship between the two schemas
- Possible values: "EQUIVALENT", "NO_RELATIONSHIP", "LEFT_IS_GENERALIZATION", "RIGHT_IS_GENERALIZATION"; 

If there is no witness, **null** is returned.
In case of request failure (HTTP status codes 400, 404, 500) an error object will be returned.

## Examples
### Request
```JSON
{
    "leftSchema": "{\n  \"const\": 123\n}",
    "rightSchema": "{\n  \"const\": 123\n}"
}
```

### Response
```JSON
{
    "timestamp": "2021-06-02T14:55:29.365847Z",
    "unixTimestamp": 1622645729365,
    "status": "OK",
    "data": {
        "leftWitness": null,
        "rightWitness": null,
        "result": "EQUIVALENT"
    }
}
```