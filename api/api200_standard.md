<img src="./imgs/zelros.svg" width="250">


# 200: Stick to the standard

## 210: Start with Swagger

API design involves writing an **API contract**. 
It declares how endpoints behave.
This is the first step of designing an API.

[Swagger](https://swagger.io) provides a syntax? based on the [OpenAPI v3](http://spec.openapis.org/oas/v3.0.3) standard, which is widely used between developers.


### 211: Write manually the definition file

Designing the API is the first step of the process. Technical and business stackholders brainstorm together to write a contract.

This contract is agreed by everyone and must not be changed without stackholders validation.

Therefore, API contract cannot be generated with future code.


### 212: Write the design in YAML, expose in JSON at root

Write the definition file in **YAML** (which is more readable than JSON).

Here is an example of `swagger.yaml`:

```
paths:
  /customers:
    post:
      summary: Adds a new customer
      requestBody:
        content:
          application/json:    # Media type
            schema:            # Request body contents
              $ref: '#/components/schemas/Customer'   # Reference to an object
            example:           # Child of media type because we use $ref above
              # Properties of a referenced object
              id: 10
              name: Jessica Smith
      responses:
        '200':
          description: OK
```

However, the exposition format is **JSON**.
Every endpoint exposes a JSON definition file, called `swagger.json` at the **root**.

```
GET https://api.fakecompany.com/swagger.json
```


### 213: Use a generated client to test the API

For end-to-end tests, the client SDK is dynamically generated. The [swagger-codegen](https://github.com/swagger-api/swagger-codegen) tool helps to generate a skeleton.

It avoids inconsistency between routes and the definition file (do not use direct HTTP call).


## 220: Fit to the basics

### 221: Requests

#### 221.1 Routes follow the business

##### Business Design

To build endpoints and routes, follow this way:
1. Routes fit to the business case.
2. Datamodel fits to the routes.
3. Software architecture fits to the datamodel.

A customer chooses a product:

```
POST /customers/{customer_id}/products/{product_id} => create a 2-way between customer and product
```


##### Avoid CRUD by default

[CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) is very usefull to manage ressources, but it should not replace the business strategy.

API should not expose only CRUD ressources and let the application decide of the strategy. 
The strategy is **promoted by the API**.

&#9989; Good way:
```
POST /customers/{customer_id}/buy/{product_id}
```

&#10060; Bad way (because CRUD is not transactional):
```
PATCH /customers/{customer_id}
PATCH /products/{product_id}
```


##### Try to nest 2 relationships maximum

Nested relationships are convenients but can become difficult to understand:

&#9989; Good way (if the ressource is not too large):
```
GET /customers/{customer_id} => get the user with addresses and streets
```

&#10060; Bad way:
```
GET /customers/{customer_id}/addresses/{address_id}/street => get the street of the address of the user
```


##### Plural

Use the plural version of a resource name, 
unless the resource in question is a singleton within the system
(for example, the overall status of the system might be /status).
This keeps it consistent in the way you refer to particular resources.

&#9989; Good way:
```
GET /customers => get all customers
GET /customers/{customer_id} => get one customer
```

&#10060; Bad way:
```
GET /customers => get all customers
GET /customer/{customer_id} => get one customer
```


##### Lowercase and snake_case

Use lowercase in path for a resource. Lowercase is compatible everywhere.

For compound noun, use snake_case (with underscore '\_ ', e.g: street_number).
So that attribute names can be typed without quotes in most languages (e.g. Javascript)

&#9989; Good way:
```
GET /insurance_customers

HTTP 200 OK
{
  "first_name": "John",
  "last_name": "Doe"
}
```

&#10060; Bad way:
```
GET /insuranceCustomers

HTTP 200 OK
{
  "first-name": "John",
  "last-name": "Doe"
}
```
  

#### 221.2: Order with HTTP verbs

HTTP request methods are not limited to GET or POST. There is a lot of [useful verbs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods), which avoid extra-layer in API design.

| Verb   | Standard action |
| ------ | --------------- |
| GET    | Read a ressource<br>Query a list of ressources|
| POST   | Add a new ressource<br>Subscribe to a ressource|
| PUT    | Replace a whole ressource|
| PATCH  | Partially replace a ressource|
| DELETE | Remove a ressource<br>Unsubscribe from a ressource|


#### 221.3: Action = Route + Verb

An action is a combination of a **route** and a **verb**.

Options like filtering, pagination, formating are set in _query string_.


&#9989; Good way (readable):
```
POST /customers => create a ressource
GET /customers/{customer_id} => get a ressource by id
GET /customers?q=Doe&from=10&count=10&format=xls => find a list of ressources
```

&#10060; Bad way (doesn't apply all posibilities of the HTTP language):
```
POST /customers/create => create a ressource
POST /customers/getbyid => get a ressource by id
POST /customers/list =>  find a list of ressources
```


#### 221.4: Prefer JSON format in request

JSON is preferred over form-encoded data.

This creates symmetry with JSON-serialized response bodies.

&#9989; Good way:
```
POST /customers
{
  "name": "John Doe",
  "age": 24
}
```

&#10060; Bad way:
```
POST /customers
name=John Doe
age=24
```


### 222: Responses

#### 222.1: Identify ressources with UUID

Give each resource an id attribute by default. Use UUIDs unless you have a very good reason not to. Don’t use IDs that won’t be globally unique across instances of the service or other resources in the service, especially auto-incrementing IDs.

&#9989; Good way:
```
GET /customers/9c4752f9-64d6-4d9a-8c37-b298bd85d1ad
```

&#10060; Bad way:
```
GET /customers/23
```


#### 222.2: Timestamps are UTC times formatted in ISO8601

Accept and return times in UTC only. Render times in [ISO8601 format](https://en.wikipedia.org/wiki/ISO_8601).

Template is `YYYY-MM-DDThh:mm:ss.000Z` with:
- `YYYY`: standard year on 4 
- `MM`: months of the year (starting with 01)
- `DD`: days of the month (starting with 01)
- `hh`: hours of the day (starting with 00 on 24-hour clock)
- `mm`: minutes of the hour (starting with 00)
- `ss`: seconds of the minute (starting with 00)
- `000`: milliseconds of the minute
- `Z`: timestamp is UTC

&#9989; Good way:
```
{
  "finished_at": "2012-01-01T12:00:00Z"
}
```

&#10060; Bad way:
```
{
  "started_at": "2012-01-01 12:00:00", // Not standard
  "finished_at": 1589037646 // Timestamp in seconds since 1970
}
```


#### 222.3: Answer with HTTP status code

HTTP responses are not limited to a 200 status code. There is a lot of [useful status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes), which avoid extra-layer in API design.

Remark:  `1xx` codes and `3xx` codes are never used (302 is only used for website or authentification).


##### Success codes

| Code | Standard action |
| ---- | --------------- |
| 200  | Search results<br/>Ressource updated |
| 201  | Ressource created |
| 204  | Ressource deleted |


##### Error codes

| Code | Standard action |
| ---- | --------------- |
| 400  | Syntax error in parameters |
| 404  | Ressource not found | 
| 429  | Too many requests (ratelimit) |
| 500  | Server throws an error |


##### Security codes

| Code | Standard action |
| ---- | --------------- |
| 401  | User disallowed |
| 403  | User allowed but ressource access disallowed |


#### 222.4: Prefer JSON format in response

JSON response is preferred over formats (like XML or TXT).

For RAW data (like PNG or JPEG), response contains directly the data, without encapsulation.
Metadata are stored in the response header.

If needed, format can be enforced with a query string parameter.

&#9989; Good way:
```
GET /persons?q=dupont&format=csv

HTTP 200 OK
Content-Range: 0-5/10
id,name
<uuid1>, Jean Dupont
<uuid2>, Nicolas Dupontel
```

&#10060; Bad way:
```
GET /persons/csv?q=dupont
```


#### 222.5: Nest foreign key relations

Serialize foreign key references with a nested object.

&#9989; Good way:
```
{
  "name": "service-production",
  "owner": {
    "id": "5d8201b0..."
  },
  // ...
}
```

&#10060; Bad way:
```
{
  "name": "service-production",
  "owner_id": "5d8201b0...",
  // ...
}
```

When nesting foreign key relations, use either the **full record** or **just the foreign keys**. 
Providing a subset of fields can lead to surprises and confusion, 
makes inconsistencies between different actions and endpoints more likely.


### 223: Misc

#### 223.1: Manage Versioning in the Accepts Header

In the request, provide the version specification in the header. A template can be:

```
Accept: application/vnd.<company>.<apiname>.json;version=<num>
```

If the version is not specified, always use the latest version.

&#9989; Good way:
```
GET /customers/{customer_id}
Accept: application/vnd.zelros.prevoyance.json;version=1
```

&#10060; Bad way:
```
GET /v1/customers/{customer_id}
GET customers/{customer_id}?apiversion=v1
```


#### 223.2: Provide Request-Id and Session-Id for Introspection

For each API response, include a `Request-Id` header, populated with a UUID value.
By logging this ID on the client, server and any backing services, it provides a mechanism to trace, diagnose and debug request scope.

For each API request, ask a `Session-Id` header, populated with a UUID value.
These ID provides a mechanism to trace a sequence of requests.

```
GET /customers/{customer_id}
Session-ID: d9c32fd5-ed70-46dd-95d2-2d4d7ed08184

HTTP 200 OK
Request-Id: 2d3b68e5-b46c-404e-8011-81f3e668a44f
{
  // Content
}
```


## 230: Usual routes

### 231: Search ressources

To search ressources,
use a **GET** method 
and return a **200** status code.
 
Provide all parameters in the **query string**.

Results are a JSON array, which contains 1 line by result.


#### 231.1: Filtered search

To filter the result with a text filter, 
use the parameter `q=`

&#9989; Good way:
```
GET /persons?q=dupont

HTTP 200 OK
Content-Range: 0-5/10
[
  { "id": "<uuid1>", "name": "Jean Dupont" },
  { "id": "<uuid2>", "name": "Nicolas Dupontel" },
  ...
]
```

&#10060; Bad way:
```
POST /persons
{
  "q": "dupont"
}

HTTP 200 OK
...
```

Filters can be combine at the same level (no nested filter).

&#9989; Good way:
```
GET /persons?name=dupont&age=gt:12
```

&#10060; Bad way:
```
GET /persons?filters={"name":"dupont","age":{"gt":12}}
```


#### 231.2: Sorted search

To sort the result on keys, 
use the parameter `sort=`.

Example:
```
GET /persons?sort=name,city:desc
```

Keys are separated with comma and order is defined with:
- `:asc` for ascending order (default if not specified)
- `:desc` for descending order


#### 231.3: Paginated search

To paginate the result, 
use parameters `offset=` and `count=`:
- `offset` is index of the first item returned by the request;
- `count` is the number of items, following the first item.

`count` should have a default value (if not specified) and also a maximum value (defined in the configuration).

Also, add a `Content-Range` header to help for the next request with:
```
Content-Range <offset>-<limit>/<total count>
```

- `offset` is the index of the first item, returned by the request;
- `limit` is the index of the last item, returned by the request;
- `total count` is the total count of items in the collection, **filters included**.

Don't forget to remember **filters** and **orders** during pagination!

&#9989; Good way:
```
GET /persons?name=dupont&offset=3&count=2

HTTP 200 OK
Content-Range: 3-5/10
[
  { "id": "<uuid4>", "name": "Sam Dupont" },
  { "id": "<uuid5>", "name": "Serge Dupont" },
]
```

&#10060; Bad way:
```
GET /persons?name=dupont&range=3-5

HTTP 200 OK
{
  "data": [
    { "id": "<uuid4>", "name": "Sam Dupont" },
    { "id": "<uuid5>", "name": "Serge Dupont" },
  ],
  "offset": 3,
  "limit": 5,
  "total_count": 10
}
```


### 232: Link or unlink 2 ressources

To link (or register/subscribe) 2 ressources together, 
use the **POST** method on the first ressource,
and return a **200** status code:

```
POST /customers/{customer_id}/buy/{product_id}

HTTP 200 OK
```

To unlink (or unregister/unsubscribe) 2 ressources, 
use the **DELETE** method,
and return a **204** status code:

```
DELETE /customers/{customer_id}/buy/{product_id}

HTTP 204 No Content
```


### 233: Do an action

To do a non-ressource operation, 
use the **POST** method
and return a **200** status code.

```
POST /customers/{customer_id}/send_invoice
```

Each operation has a dedicated route.

Do not use an [action pattern](https://en.wikipedia.org/wiki/Command_pattern):

```
POST /customers/{customer_id}/actions
{
  "action": "send_invoice"
}
```


### 234: CRUD

#### 234.1: Read a ressource

To read a ressource, 
use a **GET** method, an ID in the path, 
and return a **200** status code.

```
GET /customers/{customer_id}

HTTP 200 OK
{
  "id": "{customer_id}",
  "name": "Jean Dupont",
  "age": 24,
  "created_at": "2012-01-01T12:00:00Z",
  "updated_at": "2012-01-01T13:00:00Z"
}
```

If the ressource doesn't exist, answer with a 404 status code.


#### 234.2: Create a ressource

To create a ressource, 
use a **POST** method 
and return a **201** status code.

If the ressource is small, return the full ressource in JSON.
Otherwise, provide only the ID (never return a partial view)

Provide `created_at` and `updated_at` timestamps for resources by default:

```
POST /customers
{
  "name": "Jean Dupont",
  "age": "24"
}

HTTP 200 OK
{
  "id": "{customer_id}",
  "name": "Jean Dupont",
  "age": 24,
  "created_at": "2012-01-01T12:00:00Z",
  "updated_at": "2012-01-01T13:00:00Z"
}
```

These timestamps may not make sense for some resources, 
in which case they can be omitted.


#### 234.3: Full update of a ressource

To update all fields of a ressource, 
use a **PUT** method, the ID in the path, 
and return a **200** status code.

```
PUT /customers/{customer_id}
{
  "name": "Jean Jacques Dupont"
  "age": 42
}

HTTP 200 OK
{
  "id": "{customer_id}",
  "name": "Jean Jacques Dupont"
  "age": 42,
  "created_at": "2012-01-01T12:00:00Z",
  "updated_at": "2020-12-23T23:59:00Z"
}
```

All fields are replaced and unspecified fields are deleted.

If the ressource is small, return the whole content.

Remark: ID cannot be overrided.


#### 234.4: Partial update of a ressource

To update only some fields of a ressource, 
use a **PATCH** method, the ID in the path, 
and return a **200** status code.

```
PATCH /customers/{customer_id}
{
  "name": "Jean Jacques Dupont",
  "age": null
}

HTTP 200 OK
{
  "id": "{customer_id}",
  "name": "Jean Jacques Dupont"
  "created_at": "2012-01-01T12:00:00Z",
  "updated_at": "2020-12-23T23:59:00Z"
}
```

In this example, the field `name` is modified and the field `age` is deleted.


#### 234.5: Delete a ressource

To delete a ressource, 
use a **DELETE** method, the ID in the path, 
and return a **204** status code.

```
DELETE /customers/{customer_id}

HTTP 204 No Content
```


#### 235: Manage ratelimit

Ratelimit is calculated per account (authentification needed).

If ressource is too often consumed, API can block requests with a 429 status code.

In the response, API returns the number of remaining milliseconds before the next possible call,
in the header `X-RateLimit-Remaining`.

Example:
```
POST /customers/{customer_id}/send_invoice

HTTP 429 Too many requests
X-RateLimit-Remaining 29
```


#### 236: Smart errors

##### 236.1: Generate structured error

Generate consistent, structured response bodies on errors. Include a
machine-readable error `id` and a human-readable error `message`:

```
HTTP 500 Server error
{
  "id":      "auth_server_error",
  "message": "Exernal server is inaccessible"
}
```

Document your error format and the possible error `id`s that clients may
encounter.


##### 236.2: Limit error information in production

In production, trace detailled error only in logging system.

The request only returns an error code without details (variables content, message, etc.).

It is convenient to choice the level of information returned depending on the environment type (dev or production).


##### 236.3: Avoid cross-scripting execution in error

Remove all html tags in variables when logging error.

An attacker can inject `script` tag in value, which can be executed in the response message.
