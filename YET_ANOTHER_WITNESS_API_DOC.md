# YetAnotherWitness API documentation
This is the API documentation for the YetAnotherWitness API. Please start here if you have not worked with the API before. You will find links to all relevant information on this page.

## Table of Contents
- [YetAnotherWitness API documentation](#yetanotherwitness-api-documentation)
  - [Table of Contents](#table-of-contents)
  - [Authorization](#authorization)
  - [Endpoints](#endpoints)
    - [Compare Schemas](#compare-schemas)
    - [Generate Witness](#generate-witness)
  - [Response](#response)
    - [Success](#success)
      - [Parameters](#parameters)
        - [timestamp](#timestamp)
        - [unixTimestamp](#unixtimestamp)
        - [status](#status)
        - [data](#data)
    - [Failure](#failure)
      - [Parameters](#parameters-1)
        - [timestamp](#timestamp-1)
        - [unixTimestamp](#unixtimestamp-1)
        - [httpStatus](#httpstatus)
        - [errCode](#errcode)
        - [msg](#msg)
    - [Mapping: error code <-> error message](#mapping-error-code---error-message)
      - [Examples](#examples)
        - [Successful request](#successful-request)
        - [Failed request](#failed-request)
  - [Postman collection](#postman-collection)
    - [Pre-request scripts](#pre-request-scripts)
    - [Variables](#variables)
  - [Troubleshooting](#troubleshooting)
    - [Reading from data files](#reading-from-data-files)

## Authorization
At this stage authorization is not required.

## Endpoints
The API consists of two endpoints allowing to generate witnesses and compare two JSON Schema documents.
All request are served from [https://yetanotherwitness.ey.r.appspot.com/](https://yetanotherwitness.ey.r.appspot.com/).

### Compare Schemas
Compare two JSON Schemas regarding their relationship.

[See Details](./docs/COMPARE_SCHEMAS.md).

### Generate Witness
Generate (yet another) witness from a given JSON schema.

[See Details](./docs/GENERATE_WITNESS.md).

## Response
All responses will return a **JSON** object. Therefore it's recommended that you set the Accept header appropriately.
### Success
The HTTP status code will be set to 200 and Content-Type Header will be set to "application/json".
The response body structure is in essence the same for each endpoint.
For a successful request (HTTP status code = 200) the response will always follow this general structure:
```JSON
{
    "timestamp": "<stringTimestamp>",
    "unixTimestamp": "<unixTimestamp>",
    "status": "<status>",
    "data": "<endpoint specific JSON object>"
}
```
#### Parameters
##### timestamp
- Type: String
- Description: ISO 8601 compliant String when response was sent (UTC)

##### unixTimestamp
- Type: long
- Description: milliseconds since "The Epoche"

##### status
- Type: String
- Description: response status (currently always "OK")

##### data
- Type: JsonObject
- Description: any JSON Object. This is endpoint specific and further detailed in the description for each endpoint

### Failure
Depending on why the request either HTTP status codes 400, 404 or 500 is returned. Content-Type will be set to "application/json".
In case of a failed request the response body will always follow this general structure:

```JSON
{
  "timestamp": "<stringTimestamp>",
  "unixTimestamp": "<unixTimestamp>",
  "httpStatus": "<httpStatusCode>",
  "errCode": "<application_error_code>",
  "msg": "<error message>"
}
```
#### Parameters
##### timestamp
- Type: String
- Description: ISO 8601 compliant String when response was sent (UTC)

##### unixTimestamp
- Type: long
- Description: milliseconds since "The Epoche"

##### httpStatus
- Type: int
- Description: HTTP status code

##### errCode
- Type: int
- Description: application specific error code. Mapping errCode <-> msg

##### msg
- Type: String
- Description: application specific error message. See [Mapping errCode <-> msg](#mapping-error-code---error-message)

### Mapping: error code <-> error message
| Error code | Message                                                            |
| ---------- | ------------------------------------------------------------------ |
| 100        | Ooops something went wrong! An unexpected error occured.           |
| 101        | Ooops something went wrong! An unexpected error occured.           |
| 102        | Ooops something went wrong! Invalid Witness. An invalid witness has been generated. |
| 103        | Ooops something went wrong! Malformed JSON. Did you enter a correct JSON Schema?   |
| 104 |    Ooops something went wrong! Witness generation error. An error occurred during witness generation. |
| 105 | Ooops something went wrong! Invalid arguments. Please check provided arguments. |
| 106 | Ooops something went wrong! No handler found for path *{request path}* and HTTP Method *{request method}*. |
| 107 | Ooops something went wrong! Media type *{media type}* not supported. List of supported media types: *{list of supported media types}*. |
| 108 | Ooops something went wrong! HTTP request method *{request method}* not supported. List of supported request methods: *{list of supported request methods}*. |

#### Examples
##### Successful request
```JSON
{
    "timestamp": "2021-06-02T10:55:19.452602Z",
    "unixTimestamp": 1622631319452,
    "status": "OK",
    "data": {
        "..."
    }
}
```

##### Failed request
```JSON
{
    "timestamp": "2021-06-02T14:28:27.722931Z",
    "unixTimestamp": 1622644107722,
    "httpStatus": 500,
    "errCode": 100,
    "msg": "Ooops something went wrong! An unexpected error occured."
}
```

## Postman collection
[Postman](https://www.postman.com/) is a platform for API Development, which - among other things - allows you to conveniently document and test an API. A Postman collection therefore is just a container for requests, documentation, scripts etc. related to an API which can be shared with others to give them access to everything defined within the collection. Thus, [importing a collection](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/#importing-data-into-postman) into Postman will give you an easy way to use, test and understand the API. 
You can download a [Postman collection](./YetAnotherWitnessAPI.postman_collection.json) for the YetAnotherWitness API with sample requests, detailed documentation to each endpoint and already written pre-request scripts to easily populate the request body.

### Pre-request scripts
Postman can execute JavaScript before making a request. These so called "pre-request scripts" are here used to populate a Postman local variable called "body", which can be accessed within the request body using this syntax:
```
{{body}}
```
This variable is used by default within the tab "Body" for each request.

More details on how to set and access local variables can be found [here](https://learning.postman.com/docs/sending-requests/variables/#accessing-variables).

### Variables
Collection scoped variables allow to set values within the scope of a collection in Postman. 
Currently the folowing variables are defined:

| Name | Initial Value                              | Current Value                              |
| ---- | ------------------------------------------ | ------------------------------------------ |
| host | https://yetanotherwitness.ey.r.appspot.com | https://yetanotherwitness.ey.r.appspot.com |

You may change the variable values by clicking on a collection **...** -> **Edit** -> Tab **Variables**.

More details on Postman variables can be found [here](https://learning.postman.com/docs/sending-requests/variables/).

## Troubleshooting
If you have issues when sending requests take a look into the Postman console for more detailed information or consult the [Postman documentation for troubleshooting](https://learning.postman.com/docs/sending-requests/troubleshooting-api-requests/).

### Reading from data files
Postman allows to read values from JSON files, which may be helpful for working with the API. 

More details can be found in the [Postman documentation](https://learning.postman.com/docs/running-collections/working-with-data-files/)