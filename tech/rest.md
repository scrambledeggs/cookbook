# REST

## API Design Principles
Principles to follow when designing the API.

### Pragmatic-REST Approach

In addition to standard RESTful endpoints, we support custom actions for specific use cases. These custom actions are designed to provide additional functionality that might not fit neatly into the standard REST framework.

1. Core REST Principles:
	- Use HTTP methods correctly (GET, POST, PUT/PATCH, DELETE).
	- Utilize resource-oriented URLs.
	- Support stateless interactions.
	- Use standard HTTP status codes.
2. Pragmatic Adaptations:
	- Allow custom actions where necessary.
	- Use meaningful action names that describe the operation clearly.

#### Naming and Documenting Custom Actions

1.	Custom Actions:
	- Clearly document any custom actions.
	- Explain the purpose and behavior of each custom action.
2.	Consistent Naming:
	- Use clear and consistent naming conventions for custom actions.

## Path structure

```
.
└── v[n]/[resource]/
    ├── / (GET)
    ├── / (POST)
    ├── / (PUT/PATCH)
    ├── / (DELETE)
    └── / [id]/
    │   ├── / (GET)
    │   ├── / (POST)
    │   ├── / (PUT/PATCH)
    │   └── / (DELETE)
    └── / [custom-action]/ (POST)
```

Example
```
GET    /v1/listings                      // get all listings
GET    /v1/listings/[uuid]               // get a listing
POST   /v1/listings                      // create a listing
PUT    /v1/listings/[uuid]               // update a listing
DELETE /v1/listings[uuid]                // delete a listing
POST   /v1/listings/custom-action        // custom action for listings
POST   /v1/listings/[uuid]/custom-action // custom action for a listing
GET    /v1/custom-action                 // custom action
```

### Conventions

- Use clear and consistent naming conventions for resources and actions.
- Use kebab-case for multiple word resource or action

### Versioning

- **Prefix-Based Versioning**: Versions are indicated by prefixing the path with the version number (e.g., `/v1/resource`).
- Version is based on the action performed

**Note**: We've previously implemented versioning but had it postfixed in the path, we're depracting those. eg `listings/v1` or `listings/[uuid]/custom-action/v1`

## Request-Response Structures

### Security
:construction:

- All requests must have `X-API-KEY` header
- If user is logged in, all request must have additional `X-USER-TOKEN` header

### Requests

#### GET Requests

##### Single resource

Request: `GET /[resource]/[id]`

path parameters:
```
id                 string
```

##### Multiple resource
- Always paginate this request

Request: `GET /v1/[resource]?page=1&results_per_page=20`

query parameters:
```
page               number    optional, default to 1
results_per_page   number    optional, default to 10, max value 100
// other filters
```

Query parameters accepted value types:
- string
- boolean
- number


#### POST/PUT/PATCH Requests

Request: `POST /v1/[resource]/[id]`

JSON body:
```json
{
  "data": "to post",
  "is_true": false,
  "many": 10,
}
```

#### DELETE Requests

Request: `DELETE /v1/[resource]/[id]`

path parameters:
```
id                 string
```

### Responses

#### Single data response
- Direct to root data
```json
{
  "id": "[uuid]",
  "created_at": "[timestamp]",
}
```

Example
```json
{
  "id": "[uuid]",
  "name": "Antee Annes",
  "address": "BGC",
  "created_at": "[timestamp]",
}
```

#### Multiple data response
- wrap group of data in `results`
```json
{
  "results": [{
    "id": "[uuid]",
    "per_record": true
  }, {
    "id": "[uuid]",
    "per_record": false
  }],
  "metadata": {
    "other": "information",
  }
}
```

Example
```json
{
  results: [{
    "id": "[uuid]",
    "name": "Antee Annes",
    "address": "BGC",
    "created_at": "[timestamp]",
  }, {
    "id": "[uuid]",
    "name": "Antee Annes",
    "address": "Mandaluyong",
    "created_at": "[timestamp]",
  }],
  metadata: {
    "other_data": "something",
  }
}
```

#### Metadata

- How to include metadata in responses for pagination, filtering, and sorting.

Example
```json
{
  "results": [
    // list of resources
  ],
  "metadata": {
    "page": 1,              // FE sent (or default) reflected back
    "results_per_page": 10, // FE sent (or default) reflected back
    "total_count": 100,     // overall count of results (non-paginated)
    "max_page": 10,         // total_count / results_per_page
  }
}
```

### Error Responses

**Note**: don't show error messages that might show our database structure or the stack trace of the error

#### Single error response
```json
{
  "code": "ERROR_CODE",
  "message": "Error Message", // kind of user friendly version of ERROR_CODE
}
```

Example
```json
{
  "code": "USER_NOT_FOUND",
  "message": "User information not found"
}
```

#### Multiple error response
```json
{
  errors: [{
    "code": "ERROR_CODE_1",
    "message": "Error Message", // kind of user friendly version of ERROR_CODE
  }, {
    "code": "ERROR_CODE_2",
    "message": "Error Message", // kind of user friendly version of ERROR_CODE
  }]
}
```

Example
```json
{
  errors: [{
    "code": "INVALID_NAME",
    "message": "Name should have a letter (lol)",
  }, {
    "code": "INVALID_ADDRESS",
    "message": "Address too short",
  }]
}
```

### Conventions

- Use clear and consistent naming conventions keys 
- Use snake_case for JSON keys

### HTTP Status Codes

- **200 OK**: Standard response for successful HTTP requests.
- **201 Created**: When a resource has been successfully created.
- **204 No Content**: When a request has been successfully processed, but there is no content to return.
- **400 Bad Request**: When the payload/request is malformed or not valid.
- **401 Unauthorized**: When one of the token headers is not valid.
- **403 Forbidden**: When the user does not have the necessary permissions.
- **404 Not Found**: When the requested resource could not be found.
- **412 Precondition Failed**: When the required headers are missing.
- **500 Internal Server Error**: General fallback for any error that occurred.
- **502 Bad Gateway**: When calling to another service (either internal/external/database query) failed.
- **503 Service Unavailable**: When a direct connection to the database failed.
