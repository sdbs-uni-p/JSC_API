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
    - [How to use pre-requests scripts](#how-to-use-pre-requests-scripts)
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

### How to use pre-requests scripts
Pre-request scripts can be found within the tab "Pre-request Script" within each Postman request within the collection.
A pre-request script in this Postman collection will always comprise 4 sections:
1. Description of its purpose
2. **Definition of data/ schemas/ input**
3. Definition of body structure (according to the documentation for each enpoint)
4. Setting the "body" variable
You usually only need to be concerned with the 2nd section i. e. the definition of the schema(s) or input(s) you would like to use. The variables you are supposed to change will be marked with "Define your ..." as a comment above them. 
For instance, assume you would like to compare the following two schemas:

```javascript
{
  "const": 123
}
```
```javascript
true
```
Then you would navigate to the pre-request script of the /compareSchemas endpoint and find the section which reads "Define your input schemas here". There are two variables "leftSchema" and "rightSchema" defined there. Assign the schemas you want to compare to those two variables and you are all set to make a request to the API. 
The relevant part of the pre-request script would therefore look like this. Notice how " can be omitted when defining properties in JavaScript objects.

```javascript
//-------------------------------------
// Define your input schemas here
//-------------------------------------
const leftSchema = { const: 123 };
const rightSchema = true;
```
The whole pre-request script would look like this.
```javascript
//-------------------------------------
// Pre-request script to fill JSON body using the {{body}} Postman local variable conveniently. 
// 
// This JavaScript code will be run by Postman before the request is acutally sent to the server.
// It allows you to conveniently specify the JSON Schemas you want to compare as values for the two variables "leftSchema" and "rightSchema".
// Furthermore it will automatically convert the schemas and request body into the correct format to form a valid request.
// Using the Postman console ("Console"-Button on the bottom of your screen) you are able to see the actual Reqeust/ Response headers and bodies.
// Inspecting the request body with this method will show you the result of this pre-request script.
//-------------------------------------

// Log start of pre-request script to Postman console
console.info("Running pre-request script for endpoint /compareSchemas...");

//-------------------------------------
// Define your input schemas here
//-------------------------------------
const leftSchema = { const: 123 };
const rightSchema = true;

//--------------------------------------
// DO NOT MODIFY 
// Defining body structure
const body = {
    leftSchema: JSON.stringify(leftSchema),
    rightSchema: JSON.stringify(rightSchema)
}

// Log body
console.info("Request body generated by pre-request script.", body); 

// Set postgres variable to stringified body
pm.variables.set("body", JSON.stringify(body))

// Log end of pre-request script
console.info("Pre-request script for endpoint /compareSchemas finished.");
```
Notice how you only need to change very few lines which contain the actual relevant data you care about to send the request. The rest of the script will take care of escaping, converting the data into the correct format (valid JSON) and structure (as described in the documentation for each endpoint) to be accepted by the API. 
In this case the value of the {{body}} variable and therefore the value of the request body will be set to:

```javascript
{
  "leftSchema":"{\"const\":123}",
  "rightSchema":"true"
}
```

> Hints
> - Open the Postman console and check if the value for the request body is filled as you expected
> - Make sure the {{body}} variable is used as the request body by navigating to "Request Body"
> - JSON is valid JavaScript, therefore you can copy any valid JSON and assign it as a value to a variable
> - You can omit ' " ` when defininig properties within an JavaScript object (in contrast to JSON)
> - You can have trailing , within an JavaScript object (in contrast to JSON)

More details on pre-request script can be found [here](https://learning.postman.com/docs/writing-scripts/pre-request-scripts/)

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