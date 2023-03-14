---
title: BEX REST API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  - javascript
  - csharp

toc_footers:
  - <a href='https://bex.co.za/signup'>Open a BEX account</a>
  - <a href='https://bex.co.za/quick-track'>Track a shipment</a>
  - <a href='https://bex.co.za/about'>About us</a>


includes:
  - login
  - tracking
  - quoting
  - waybill
  - webhooks

search: true
---

# Welcome to our DOCS site

Here at BEX we frequently develop and publish new API’s to assist our customers who wish to gain deeper control and visibility into their courier processes.

These API’s allow for direct integration into our courier platform, allowing data to be read and created using requests in JSON format.
Couple this with our token based Authentication methods and standard HTTP verbs (which are understood by most HTTP clients) you have the capability to orchestrate and manage a number of processes within the courier environment.

We hope that you will build something great and benefit from integrating with our courier API’s.

We want your courier journey to be pain-free - If you run into any difficulties, or perhaps you have some suggestions for us, please do not hesitate to <a href="mailto:it@bex.co.za?subject=Please%20help%20me%20to%20integrate%20with%20you">reach out</a> and we will gladly work with you.

# Requirements

To make use of our API integration we require the following:

1. A valid <a href="https://bex.co.za/signup">shipping account</a> with BEX.
1. The creation of an integration user identity under which you will transact over the API’s
1. For security sensitive data requests, a valid <a href="#login">token</a>.

**Note:** If you have multiple shipping accounts you are not required to transact under multiple integration identities (“tokens”). Our platform supports the assignment of multiple shipping accounts to a _single_ token, making integration across multiple business units easier.

<aside class="notice">
You may access the API over HTTP or HTTPS, but HTTPS is recommended if you wish to keep things secure.
</aside>

# Request/Response Format
The default response format is **JSON**. Requests with a message-body use plain JSON to set resource attributes.

Successful requests will return a `200 OK` HTTP status.

# Errors

You can find the errors specific to each API endpoint under its dedicated API topic.

We communicate API errors in the _ex:_ ( _ex_ ception ) attribute found in the base object of our API responses.

**A word on success flags:**
We return `200 OK` http status responses for **ALL** requests that make it to our server, **even if we run into an error when processing your request.**

For API submissions that result in a processing error (read _validation and business logic/”elegant” type errors_) we include an attribute in the API response titled _ex:_ where we communicate the reason for the processing failure.

A response could look as follows:

* HTTP status code: `200 OK`
* API response: `"ex": "Account number is not registered for transactions with this token."`

![200 OK with error ex:](200OK-response-with-ex-error.jpg)
_Above: Note the **200 OK** status whilst receiving an error **"ex":** response_

In instances where there is a technical breakdown in the processing of the API request, such as querying the wrong endpoint address, you will receive the appropriate http status code such as a `404 error`.

# Parameters

Almost all endpoints accept optional parameters which can be passed as an HTTP query string parameter, e.g. `GET /getcustomquicktracking_V3?ref=invoices`

Parameters containing special characters that are passed in a URL request must have their data url encoded.

All parameters are documented along each endpoint.

# Libraries
We do not currently offer any libraries for our API implementation. We are in the process of creating .NET wrappers that you can use in the future. We will publish more information on this topic once it is ready for use.

# Tools
Some useful tools you can use to access the API's include:

* <a href="https://insomnia.rest">Insomnia</a> - Cross-platform GraphQL and REST client, available for Mac, Windows, and Linux.
* <a href="https://getpostman.com">Postman</a> - Cross-platform REST client, available for Mac, Windows, and Linux.
* <a href="https://requestbin.com">RequestBin</a> - Allows you test webhooks.
* <a href="https://hookbin.com">Hookbin</a> - Another tool to test webhooks.


# Authentication
Authentication against the BEX API ecosystem is a two-part process and is required to prevent access to confidential client data. The process involves the generation of your API security token which is described in the next section. This token is then included in the HTTP headers of your API calls and serves to identify and authenticate you on our platform.

API’s that serve non-sensitive client data such as our quick waybill <a href="tracking">tracking</a> do not require your token to be present in the API call and can be called anonymously.