# Endpoint: Generate (yet another) witness (Path: /generateWitness)
Generate a witness for a given JSON Schema. If unprecedented witnesses should be generated, already generated witnesses have to be provided as string within the previousWitnesses array.
For a more general overview of the API please see the [start page of the API documentation](./YET_ANOTHER_WITNESS_API_DOC.md).

## Table of Contents
- [Endpoint: Generate (yet another) witness (Path: /generateWitness)](#endpoint-generate-yet-another-witness-path-generatewitness)
  - [Table of Contents](#table-of-contents)
  - [Request](#request)
    - [Request Headers](#request-headers)
    - [Request Body](#request-body)
  - [Response](#response)
    - [Response Headers](#response-headers)
    - [Response Body](#response-body)
  - [Examples](#examples)
    - [Generate 1st witness](#generate-1st-witness)
      - [Request](#request-1)
      - [Response](#response-1)
    - [Generate yet another witness](#generate-yet-another-witness)
      - [Request](#request-2)
      - [Response](#response-2)

## Request
### Request Headers
Header Content-Type should be set to "application/json".
### Request Body
The request body generally conforms to the following structure:
```JSON
{
    "input": "String; Stringified JSON Schema",
    "previousWitnesses": [
        "String; Stringified witness 0",
        "String; Stringified witness 1",
        "..."
    ]
}
```
If array previousWitnesses does not contain any Schemas, the **first witness** will be generated.
If there is no witness, **null** is returned.

## Response
### Response Headers
Content-Type will be set to "application/json" and HTTP status code will be set to either 200, 400, 404 or 500.
### Response Body
For a successful request (HTTP status code = 200) the expected result will look like this:
```JSON
{
    "timestamp": "String; ISO-8601 representation of time in UTC",
    "unixTimestamp": "Long; Milliseconds since the epoch",
    "status": "String; as of now always returns \"OK\"",
    "data": {
        "witness": "String; Stringified witness"
    }
}
```
In case of request failure (HTTP status codes 400, 404, 500) an error object will be returned.

## Examples

### Generate 1st witness
#### Request
```JSON
{
    "input": "{\n  \"const\": 123\n}",
    "previousWitnesses": [ ]
}
```

#### Response
```JSON
{
    "timestamp": "2021-06-02T10:55:19.452602Z",
    "unixTimestamp": 1622631319452,
    "status": "OK",
    "data": {
        "witness": "123"
    }
}
```

### Generate yet another witness
To generate another witness pass previous witnesses in the previousWitnesses array.
If there are no more witnesses, **null** is returned.
#### Request
```JSON
{
    "input": "{\n  \"const\": 123\n}",
    "previousWitnesses": [ 
        "123"
    ]
}
```

#### Response
```JSON
{
  "timestamp": "2021-06-02T10:46:55.853200Z",
  "unixTimestamp": 1622630815853,
  "status": "OK",
  "data": {
    "witness": null
  }
}
```