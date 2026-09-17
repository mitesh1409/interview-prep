# REST APIs

Lesson: [Deep Dive into REST API Design and Implementation Best Practices](https://www.youtube.com/watch?v=7nm1pYuKAhY)

## Naming

#1 Use nouns to represent resources, not verbs.  

Examples:  

Good  

https://www.example.com/api/v1/store/items/{id}

https://www.example.com/api/v1/store/employees/{id}


Bad  

https://www.example.com/api/v1/store/getItems/{id}

https://www.example.com/api/v1/store/getEmployees/{id}

---

#2 Leverage logical grouping by reflecting object relationship.  

Examples:  

Get the orders for a customer  
https://www.example.com/api/v1/customers/99/orders

Get the customer for an order  
https://www.example.com/api/v1/orders/1001/customers

Please note that while using this API endpoint structure we expose  
the relationship between objects to the outside world.  

---

#3 Use pluralized nouns for resources.  

Examples:  

Good  

https://www.example.com/api/v1/store/items/{id}

https://www.example.com/api/v1/store/employees/{id}

Used pluralized nouns - items, employees.  

Bad  

https://www.example.com/api/v1/store/item/{id}

https://www.example.com/api/v1/store/employee/{id}

Used singularized nouns - item, employee.  

---

#4 Collection is a group of resources.  

A resource has data and relationships to other resources.  
A group of resources is called a collection.  

/orders - it is a collection of orders.  

/orders/99 - a resource with information about a specific order.  

---

#5 Use hyphens `-` to improve readability, don't use underscores `_`.  

---

#5 Don't forget to properly version your APIs.  

---

## HATEOAS

---

## Filtering, Sorting & Pagination

#1 Filter data by specific key-value.  

Example:  

https://www.example.com/api/v1/store/customers?firstname=John&age=63

https://www.example.com/api/v1/store/orders?status=Completed

#2 Fetch only specific fields by key.  

Example:  

https://www.example.com/api/v1/store/orders?fields=id,total_amount,status,completed_at

#3 Limit the number of items for the data returned.  

https://www.example.com/api/v1/store/customers?limit=50

#4 To paginate the data chunk by chunk instead of querying all at once.  

https://www.example.com/api/v1/store/customers?start=0&limit=50

#5 To sort the data by a specific key-value.  

GET /api/v1/store/books?sort=author,-date_published

Get books sorted by author in ASC order and date_published in DESC order.  

Convention:  

* No prefix = ascending (default)
* `-` prefix = descending
* Comma-separated = multiple sort keys, in priority order

---

## Idempotency

#1 Route controllers should not rely on side-effects.  

#2 Same request repeated for the same resource should result in same state.  
HTTP status codes in the response may differ.  

For example, sending multiple delete requests to the same URL should have  
the same effect.  

Although the HTTP status code and the response messages may different,  
that's totally normal.  

First delete request  
DELETE https://www.example.com/api/v1/store/customers/99  
Response - 204 No Content  

Second delete request to the same resource  
DELETE https://www.example.com/api/v1/store/customers/99  
Response - 404 Not Found  

> Make sure your controller methods are pure functions.

---

## Async Operations

Sometimes an API might require long time to complete.  

If server waits for completion before sending a response to the client,  
it will increase latency and result in poor responsiveness.  

We can make such operation asynchronous.  

Server can return HTTP status code 202 Accepted,  
to indicate that the request was accepted for processing,  
but not yet completed.  

Server then can expose an endpoint that returns the status  
of the asynchronous request, so the client can monitor the  
status by polling the status endpoint.  

Also include the URL of the status endpoint in the "Location"  
header of the 202 Accepted response.  

When client sends GET request to this status endpoint,  
the response should contain the current status of the request.  
Optionally, it could also include an estimated time to completion,  
or a link to cancel the operation.  

Example:  

A logged in user requests for quarterly report.  

Request:  
GET /quarterly/report  

This request takes long time to complete.  
So we handle it asynchronously.  

Response:  
202 Accepted  
This indicates - server accepted the request, it is in progress...

Location: GET /quarterly/report/status  
This is the status endpoint in the Location header of the response.  
Client can monitor the status of the asynchronous request  
by polling the status endpoint.  

Request:  
GET /quarterly/report/status  

When client sends GET request to this status endpoint,  
the response should contain the current status of the request.  
Optionally, it could also include an estimated time to completion,  
or a link to cancel the operation.  

Response:  
200 OK  

```json
{
    "status": "In Progress",
    "timeToComplete": 3000,
    "cancel": {
        "method": "DELETE",
        "route": "/quarterly/report",
    }
}
```

If the asynchronous operation creates a new resource,  
the status endpoint should also return the status code 303 See Other  
after the operation completes.  

In the 303 See Other response, include a "Location" header that  
gives the URL of the new resource.  

---

## Partial Responses

Large binary fields, like files or images, may be included in a resource.  

To improve response times and overcome issues with intermittent connections,  
we need to enable the retrieve of these resources in chunks.  

To do so, the API should support the "Accept-Ranges" header for GET requests of  
large resources.  

This header allows partial requests for specific byte ranges of resource,  
which can be submitted by the client application.  

Also consider implementing HTTP HEAD request for those resources.  
A HEAD request is similar to a GET request, except that it only returns  
the HTTP headers that describe the resource with an empty message body.  

A client application can issue a HEAD request to determine whether to fetch  
a resource by using partial GET request.  

Example:  

Request:  

HEAD https://www.example.com/api/v1/files/99  

Response:  

200 OK  

Accept-Ranges: bytes  
Content-Type: image/jpeg  
Content-Length: 4580  

The "Content-Length" header here gives the total size of the resource,  
and the "Accept-Ranges" header indicates that the corresponding GET operation  
supports partial content.  

The client application can use this information to retrieve the image/file  
in smaller chunks.  

Then, the first request fetches the first 2500 bytes by using the "Range" header.  

Request:  

GET https://www.example.com/api/v1/files/99  

Range: bytes=0-2499

The reponse message indicates that this is a partial response by returning  
an HTTP status code 206 Partial Content.  
The "Content-Length" header specifies the actual number of bytes returned  
in the message body, and the "Content-Range" header indicates which part  
of the resource this is.  

Response:  

206 Partial Content

Content-Range: bytes 0-2499/4580
Content-Length: 2500

A subsequent request from the client application can retrieve the remainder  
of the resource, just very similarly to pagination.  

Request:  

GET https://www.example.com/api/v1/files/99  

Range: bytes=2500-4580

---

## Error Handling

#1 Endpoint users need to know what went wrong to be able to fix the request.  

#2 Make sure you don't expose the internal mechanics of the controller.  

#3 Make sure to return the correct HTTP status codes in case of failures.  
Client error responses - 400 to 499  
Server error responses - 500 to 599  

---

When the body of a successful response is empty,  
the status code should be 204 No Content instead of 200 OK.  

---

## Security

#1 Make SSL/TLS encryption a default for your API endpoints.  
Use HTTPS protocol for your API endpoints.  

#2 Authentication & Authorization  

#3 Implement ACLs (Access Control Lists)  

#4 Rate Limiting  

#5 Throttling  

#6 IP Blacklisting  

#7 Preventing XSS  

---

## Documentation

Use OpenAPI for documenting APIs.  
