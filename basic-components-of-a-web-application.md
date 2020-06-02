# Basic Components of a Web Application

Terms

- Client App (could refer to Web Browser, Mobile, Desktop, Console or Other client consuming application)
- Browser - Web Browser
- WebPage - full webpage with html
- API - API Endpoints

There are several basic components that every web application should have:

- Authentication & Access control
- Caching
- Logging
- Exception handling
- Validation

## Authentication
Security in a web application is often overlooked, yet forms the most basic component every app should have. 
Security is primarily a backend thing, with the frontend providing different views, layouts, navigation. sometimes developers get this wrong and build the security from a frontend first, it should be serverside first as this is what is handling data storage and processing.

- Authentication - the process of logging in, obtaining a token/cookie, authkey etc
- Authorization - often used interchangeably with Authentication, the process of validating a login token, authkey for a user etc.
- Access control - defining the permissions and access the anonymouse/authenticated user may have in relation to resources

Authentication typically consists of requestng a token from an authorization server, and storing the token in the client app, whether it be a web browser as a cookie (or token), or mobile or desktop application token. There are different ways to obtain a token which are explained in more detail (ill provide links soon) and various mechanisms for doing so. OpenID, JWT bearer etc.

Once we have a token somewhere, everytime we make a request to a web server endpoint with a webpage through a web browser, any cookies set for that domain are automatically sent up to the server. 

In the case of an actual _token_ which is a string containing a cryptographic signature and identifies the authenticated user, we would send this up in the _Authorization_ request header, this could be from a browser, mobile, desktop or any other kind of app (if there is any lol). 

If all is well, we make a request with the token and get what we need from the server, if for any reason the token is no longer valid, ie it's expired, or invalid signature, than the API would return a **HTTP Status 401 (Unauthorized)**, this lets the client app know we need to obtain a new token, by logging in again, or using a refresh token (more on this later).

Now, some forms of authentication may use redirects in the browser in order to obtain a cookie/token, however should always return the appropriate **HTTP 401** if calling an api using an invalid token to ensure the client app knows it's an authentication issue. This can be acheived by checking the request accepts headers, for instance if the client app accepts **"application/json"** and the request **"Authorization"** header is set, you can be pretty sure it's a call that wont handle **"text/html"** as a redirect response (used for redirecting to a logon page or similar third party auth).... for one the client app wont have a clue what the actual error is.

## Caching
Now there are several forms of caching

- Browser caching (css/js and other static assets)
- Client app caching (request caching, offline storage)
- Distributed Caching (such as Redis)
- Memory caching (server caching)

## Logging

Logging info, debug, warnings and errors
This can be exceptionally useful for reviewing, and especially helpful when it comes to diagnosing issues that happen only in certain environments, logging should be setup according to the severity.

## Exception Handling

Global exception handling to gracefully report and return to the client something unexpected has happened. This is typically something you want to do at a configuration / startup level, and not catching in every method or action call, define the handler once if possible, and return the same error structure to the client.

## Validation

Server side validation should always come first, over client side validation, as a front end client can easily be modified, especially if it's a browser based application (html/css/js). Validation errors with forms should return **400 Bad Request** with identifiers of the form fields and errors. 

Also, a highly recommended approach of returning errors of any kind to a client is to use a standardized structure, for instance Problem details from IETF [Problem Details Spec](https://tools.ietf.org/html/rfc7807), in the case of a *400* you would provide a dictionay of errors in the errors object with each key (possibly a form field) with an array of strings.

ie

```js
{
    "errors": {
        "firstName": [
            "required",
            "cannot contain numbers"
        ]
    }
}
```

