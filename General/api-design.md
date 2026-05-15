
How to design APIs like a senior engineer  
https://www.youtube.com/watch?v=rU2T0Y--LG4


REST API

Easy to lie.  
HTTP Status 200 OK, but it may have some problem.
HTTP Status 404 Not Found, we may have a requested resource but we still return 404.  

Not recommending REST for building your private APIs.  
For building public APIs, REST is good choice because it is a standard that everyone follows and understand.  

---

Proprietary architecture for building your private APIs  

#1  
Deciding the API end-point URL  

Option #1  
POST www.example.com/api

Batching API calls possible  

OR

Option #2  
POST www.example.com/api/one  
POST www.example.com/api/two  
POST www.example.com/api/three  

Batching API calls not possible  

@todo  
Explore about batch requesting APIs from client side.

#2  
Lets go with Option #1  
POST www.example.com/api  

We chose this because batch request is possible with this option.  

All the APIs have this single end-point.  

POST request body looks like:  
{
    "api_name": "abcd",
    "payload": {
        // post data here
    }
}

request headers like:  
x-access-token (JWT signed token)  
cookie (http only)  
etc.  

Just a single API URL, POST request always.  
We will pass data/payload when required, else it will be empty.  

Some of the sample API end-points,  
POST www.example.com/api  
    /health  
    /sign-up  
    /login  
    /logout  
    /products  
etc. APIs

#3  
API Request Flow  

Client --> Load Balancer/WAF --> API Server --> Input Validation --> Rate Limiting

#4  
@todo  
Explore about Load Balancer/WAF  

#5  
API Server - it can be VM or Lambda  

#6  
Input Validation  
Validating user input  

How do you validate post request body?  

POST www.example.com/api  
    /sign-up  
    /login  
    /logout  
    /products  
etc. APIs

There is an "api" folder and each of the API has its own folder.  
Also each api has its own handler/controller and constants/meta-data files.  

constants/meta-data  
It can have  

- configs for rate limiting
- configs specific validations
- input/output or request/response schema (using https://zod.dev/) for type safety
etc.

Folder structure looks like:  

```plaintext
api
|
|-- sign-up
|       |
|       |-- handler/controller <-- this handles the API request, main file
|       |-- constants/meta-data <-- API related meta-data, rate limiting, specific validations etc.
|
|-- login
|
|-- logout
|
|-- products
```

#7  
Rate Limiting  

Authorize people for sensitive actions.  
In other words, protect your sensitive/important APIs by providing access to only authorized users.  

Rate Limiting based on IP is not a good solution,  
because multiple users can share the same IP,  
for example college/university users, office/workspace users etc. they are behind a single network.  

if unauthorized request then
    rate limit based on an IP

if authorized request then
    rate limit based on account ID

Also consider to rate limit at an API level.

#8  
Once we are through the Rate Limiting step,  
execute the handler/controller code.  

First step inside every handler/controller must be Authentication + Authorization.  

Please note that not every API will require Authentication + Authorization,  
but whichever API need this it should be the first thing done.  


