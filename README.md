Bugsnag API
===========

Bugsnag provides a simple JSON-based API to access information about your
Bugsnag account, project and errors.

[Bugsnag](http://bugsnag.com) captures errors in real-time from your web, 
mobile and desktop applications, helping you to understand and resolve them 
as fast as possible. [Create a free account](http://bugsnag.com) to start 
capturing exceptions from your applications.


Contents
--------
-   [Schema](#schema)
-   [Authentication](#authentication)
-   [Parameters](#parameters)
-   [Errors](#errors)
-   [Hypermedia URLs](#hypermedia-urls)
-   [Pagination](#pagination)
-   [Rate Limits](#rate-limits)


Schema
------

- API access is over HTTPS using the endpoint https://api.bugsnag.com
- Data is sent and received using JSON
- Timestamps are returned in ISO 8601 format: `YYYY-MM-DDTHH:MM:SSZ`


Authentication
--------------

All requests to the Bugsnag API require authentication. To authenticate APi requests you'll need to use an API token, which you can find on your Bugsnag "Account Settings" dashboard under the "API" section.

There are two ways to send the API token with requests, you can send the token in an `Authorization` header:

```shell
$ curl -H "Authorization: token YOUR-TOKEN-HERE" https://api.bugsnag.com
```

Alternatively, you can authenticate using a parameter:

```shell
$ curl https://api.bugsnag.com/?auth_token=YOUR-AUTH-TOKEN-HERE
```

The `Authorization` header is the preferred method of authentication.


Parameters
----------

Many API methods take optional parameters. For GET requests, any parameters not specified as a segment in the path can be passed as an HTTP query string parameter.

For POST, PUT, and DELETE requests, parameters not included in the URL should be encoded as JSON with a Content-Type of ‘application/json’.


Errors
------

- Sending invalid JSON will result in a `400 Bad Request` response
- Sending the wrong type of JSON values will result in a `400 Bad Request` response
- Trying to access the API without authenticating or with an invalid auth token will result in a `401 Unauthorized` response


Hypermedia URLs
---------------

All API resources may have one or more `*_url` properties linking to other resources. These are meant to provide explicit URLs so that API clients don’t need to construct URLs on their own. It is highly recommended that API clients use these. Doing so will make future upgrades of the API easier for developers.


Pagination
----------

Pagination info is included in the [Link header](http://tools.ietf.org/html/rfc5988). It is important to follow these Link header values instead of constructing your own URLs since the pagination method can differ between resources.

The URL for pagination is shown inside angled brackets, and the type of pagination link is described in the `rel` field:

```http
Link: <https://api.bugsnag.com/account/projects?offset=51f42cc7b2db42c554000086>; rel="next"
```

The possible values for `rel` are:

- `next` - Shows the URL for the next page of results
- `prev` - Shows the URL for the previous page of results