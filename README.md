# API
API (Application Programming Interface) is an interface to an application designed for other  computer systems to use.
APIs can be used on web-based systems, operating systems,  database systems and computer hardware.
API testing is to check whether the API meets expectations in terms of  functionality, reliability, performance, and security of an  application

# REST
REST (Representational State Transfer) is an architectural style for designing networked applications. It relies on a stateless, client-server communication protocol, typically HTTP. RESTful applications use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources.

![image](https://github.com/user-attachments/assets/8bfb4828-005f-45ec-88e1-f367bd5e2893)

# HTTP Basic Concepts
## HTTP Request
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

4. Request body or payload
- POST and PUT can have payloads, GET and DELETE can not.

## HTTP Response
1. HTTP Status Code

Send the right HTTP Status Code in response.

- 1xx: Informational
- 2xx: Successful Responses: The codes indicate that the request was successfully
  - 200 OK: The request was successful, and the server returned the requested resource.  In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request (e.g: GenerateToken), the response will contain an entity describing or containing the result of the action.
  - 201 Created: The request was successful, and a new resource was created as a result.
  - 204 No Content: The request was successful, but there is no content to return.
- 3xx: Redirection
  - 302 Temporary Redirect
- 4xx: Client Error Responses: The codes indicate that there was an error with the request made by the client.
  - 400 Bad Request: The server could not understand the request due to invalid syntax.
  - 403 Forbidden: The client does not have access rights to the content.
  - 404 Not Found: The server cannot find the requested resource.
- 5xx: Server Error
  - 500 Internal Server Error

To handle errors effectively, we need to implement exception handling that assigns the appropriate error codes. For example:

- 404 Not Found for EntityNotFoundException
- 400 Bad Request for IllegalArgumentException

2. Response body or payload: JSON or XML
3. HTTP Response Headers
- HTTP status code
- type of content
- datetime..
