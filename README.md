# API
<details>
API (Application Programming Interface) is an interface to an application designed for other  computer systems to use.
APIs can be used on web-based systems, operating systems,  database systems and computer hardware.
API testing is to check whether the API meets expectations in terms of  functionality, reliability, performance, and security of an  application
</details>

# REST
<details>
REST (Representational State Transfer) is an architectural style for designing networked applications. It relies on a stateless, client-server communication protocol, typically HTTP. RESTful applications use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources.

![image](https://github.com/user-attachments/assets/8bfb4828-005f-45ec-88e1-f367bd5e2893)

</details>

# HTTP Basic Concepts
## HTTP Request
<details>
1. HTTP Methods

- GET: Use GET requests to fetch data from the server without modifying it.
- POST: Use POST requests to create new resources.
- PUT: Use PUT requests to update an existing resource or create a resource if it does not exist.
- DELETE: Use DELETE requests to delete a resource.
- PATCH: Use PATCH requests to update a piece of resource.

Note: And GET, PUT, DELETE should be idempotent.

2. URL, Path Parameter, Query Parameter

- Path Parameters: are used to identify specific resources.

  - Example: we have user id is 123 then we should use path parameter for URL.

- Query Parameter: are used to filter, sort, or paginate resources. They are appended to the end of the URL and follow a question mark (?).

  - Example: /users?sort=age&limit=10&page=2

3. HTTP Request Headers
- type of Browser
- type of content
- what type of response is accepted in return
- Authentication
  - Public Request (no auth)
  - Basic Authentication Headers
  - Bearer Authentication Headers
  - Custom Headers
  - Session Cookies

Most common request headers 
- `Accept`, which defines the media types that the client is able to accept from the server;
- `Authorization`, which is used to send the client’s credentials to the server when the client is attempting to access a protected resource;
- `Content-Type`, which defines the media type in the request body.

4. Request body or payload
- POST and PUT can have payloads, GET and DELETE can not.
</details>

## HTTP Response
<details>
1. HTTP Status Code

Send the right HTTP Status Code in response.

- 1xx: Informational
- 2xx: Successful Responses: The codes indicate that the request was successfully
  - 200 OK: The request was successful, and the server returned the requested resource.  In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request (e.g: GenerateToken), the response will contain an entity describing or containing the result of the action.
  - 201 Created: The request was successful, and a new resource was created as a result.
  - 204 No Content: The request was successful, but there is no content to return. This code is often used in response to DELETE requests.
- 3xx: Redirection
  - 302 Temporary Redirect
- 4xx: Client Error Responses: The codes indicate that there was an error with the request made by the client.
  - 400 Bad Request: The server could not understand the request due to invalid syntax.
  - 403 Forbidden: The client does not have access rights to the content.
  - 404 Not Found: The server cannot find the requested resource.
- 5xx: Server Error
  - 500 Internal Server Error: The server encountered an unexpected issue.
  - 503 Service Unavailable: The server is not ready to handle the request, often due to overload or maintenance.
  - 504 Gateway Timeout: The server acting as a gateway or proxy did not receive a timely response from the upstream server.

To handle errors effectively, we need to implement exception handling that assigns the appropriate error codes. For example:

- 404 Not Found for EntityNotFoundException
- 400 Bad Request for IllegalArgumentException

2. Response body or payload: JSON or XML
3. HTTP Response Headers
- HTTP status code
- type of content
- datetime..

The server also includes HTTP headers in its response to the client. Some of the most common response headers include 
- `Cache-Control`, which defines how the response should be cached,
- `Set-Cookie`, which instructs the client to store a cookie with the specified attributes.
</details>

# What is the difference between the PUT and PATCH methods?
<details>
The PUT and PATCH methods are both used to modify resources, but they handle updates differently. The PUT method works by replacing a resource with the data that is included in the request body. This means that any fields or properties not included in the request body are deleted, and any new fields or properties are added.

In contrast, the PATCH method is used for making partial updates to a resource. For instance, if you have a movie resource with fields for title, year, and rating, but you only want to update the rating, you could use the PATCH method to send a request that only includes the new value for the rating field. The rest of the resource would remain unchanged. This behavior reduces the amount of data that must be transferred, which makes PATCH more efficient than PUT.
</details>

# API Testing Checklist
<details>
1. **Authentication and Authorization Tests**
- Ensure API responds to correct authorization via all agreed auth methods — Bearer token, cookies, digest, etc. — as defined in spec
- Ensure API refuses all unauthorized calls

2. **Positive Scenarios: (With valid required parameters)**
- Verify the HTTP status code for successful request

Get Request — 200 , Successful (GET: The resource has been fetched and transmitted in the message body.)
Post Request — 201 ,Created (The request succeeded, and a new resource was created as a result.). 200 status code may be used in some case.
Patch/PUT — 200 , Successful(The resource describing the result of the action is transmitted in the message body.)
Delete — 204 , (No Content ) status code if the action has been enacted and no further information is to be supplied.). 200, or 202 status code may be used in some case.

- Response Validation

Validate that the response is valid JSON object
Validate the response schema, i.e. field names, field types and field values are expected including nested objects. (In case of field values, non-nullable fields should not be null)

- Application State Validation

Verify idempotence (NO STATE CHANGE) in the system for GET requests
After performing POST,PUT,PATCH,DELETE operations, verify the new state of the application **by performing the GET request and inspecting response OR through UI OR DB**

- Headers Validation

Verify that HTTP headers are as expected, including content-type, connection, cache-control, expires, access-control-allow-origin, keep-alive, HSTS, and other standard headers are as per the specifications
Validate content-typeon request headers to allow only your supported format(e.g., application/xml, application/json, etc.) and respond with 406 Not Acceptableresponse if not matched.
Verify that the information is not leaked via response headers (e.g. X-Powered-By header is not sent to user)

- Performance Validation

Verify response time for the API request is expected. Timeout error should not be observed

3. **Extended Positive Testing with optional Parameters**
3.1. Validate the status code, application state, headers, performance as in the Positive Scenarios test by running the same tests including optional parameters in API requests

3.2. Response Validation:

Validate the response is valid JSON and schema validation
Validate the response is structured as per the optional parameter provided in the API call
Verify the expected response for all combinations of optional fields in request body/optional query parameters (filter+skip+sort+limit)


4. **Negative Scenarios (With valid input)**
API requests with valid input trying to perform illegal operations such as:
— Creating resources with name of the existing resource
— Deleting resources that doesnot exits
— Deleting,creating,updating user configuration without permission
— Updating fields that are not allowed (such as renaming resource)
— Retrieving information of resources that do not exist
— Attempting API request that are not authorized

- Validate HTTP status code

Verify that the error code is as per the error case:
a. 400(Bad request)– The server will not process the request due to the error from the client’s end. Example: invalid request URL.
b. 401(Unauthorized)– 401 indicates that the client tried to operate on a protected resource and is unauthorized.
c. 403(Forbidden)– The client sends the correct request but the server refuses to give the response.
d. 404(Not found)– The requested resource could not be identified at present and may be available in the future.
e. 405(Method not allowed)– The client uses an invalid request method that the resource doesn’t allow.
f. 500(Internal server error)

- Validate Response body

Verify that error response is received
Verify that error format is according to spec. e.g., error is a valid JSON object or a plain string (as defined in spec)
Verify that there is a clear, descriptive error message/description field
Verify error description is correct for this error case and in accordance with spec

- Validate Headers

- Validate performance/response time is as expected

5. **Negative Testing (With invalid input)**
Validate status code, response, headers, and response time for the API calls with invalid input/request. Possible cases are listed below:

- Missing or invalid authorization token
- Missing required parameters
- Invalid value for endpoint parameters, such as invalid uuid/id in path or query parameter
- Invalid request body schema
- Missing fields or nested entities in request body
- Invalid values in nested entity fields
- Invalid values in HTTP headers
- Unsupported methods for endpoints

- Malformed content in request
- Wrong content-type in request body/payload
- Content with wrong structure
- Overflow parameter values. e.g. Creating user with name longer than 50 characters, attempt to GET user with invalid UUID longer than 500 characters
- Overflow payload ⇒ huge JSON request body
- Empty request body/payloads
- Empty sub-objects in payload
- Boundary value testing
- Illegal characters in parameters or payload
- Using incorrect HTTP headers (e.g. Content-Type)
- Small concurrency tests — concurrent API calls that write to the same resources (DELETE + PATCH, etc.)
- Invalid path, incorrect field names
</details>
