# REST
REST (Representational State Transfer) is an architectural style for designing networked applications. It relies on a stateless, client-server communication protocol, typically HTTP. RESTful applications use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources.

# HTTP Methods

GET: Use GET requests to fetch data from the server without modifying it.
POST: Use POST requests to create new resources.
PUT: Use PUT requests to update an existing resource or create a resource if it does not exist.
DELETE: Use DELETE requests to delete a resource.
PATCH: Use PATCH requests to update a piece of resource.
Note: And GET, PUT, DELETE should be idempotent.

# URL, Path Parameter, Query Parameter

- Path Parameters: are used to identify specific resources.

  - Example: we have user id is 123 then we should use path parameter for URL.

- Query Parameter: are used to filter, sort, or paginate resources. They are appended to the end of the URL and follow a question mark (?).

  - Example: /users?sort=age&limit=10&page=2

# HTTP Status Code

Send the right HTTP Status Code in response.

- 2xx: Successful Responses: The codes indicate that the request was successfully
  - 200 OK: The request was successful, and the server returned the requested resource.
  - 201 Created: The request was successful, and a new resource was created as a result.
  - 204 No Content: The request was successful, but there is no content to return.
- 4xx: Client Error Responses: The codes indicate that there was an error with the request made by the client.
  - 400 Bad Request: The server could not understand the request due to invalid syntax.
  - 403 Forbidden: The client does not have access rights to the content.
  - 404 Not Found: The server cannot find the requested resource.

To handle errors effectively, we need to implement exception handling that assigns the appropriate error codes. For example:

- 404 Not Found for EntityNotFoundException
- 400 Bad Request for IllegalArgumentException
