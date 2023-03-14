# Login

## Overview

The *login* service serves as the starting point from which a valid session token will be generated. You call this endpoint to authenticate your _username_ and _password_ user credentials.

The BEX platform will validate your request and if successful, generate and return in the response object a token contained in the `"value":` attribute.

This token is unique to your integration user identity and is to be kept private as it is the key needed to unlock access to the account security restricted API’s.

<aside class="notice">
The token will not expire so once generated can be persisted securely on your side.
</aside>

## Endpoint

Service Relative URL: `api/service/login`

> Make sure to replace `user123`and `pass123` with your integration account details.

```javascript
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>BEX API Login Example</title>
<script src="http://code.jquery.com/jquery-latest.min.js" type="text/javascript"></script>
</head>
<body>
<script type="text/javascript">
    var user = encodeURIComponent('user123'); //encode the username
    var pass = encodeURIComponent('pass1234'); //encode the password
    jQuery.support.cors = true; //this will enable cross domain access for all services
    //call the service using jQuery ajax and passing the parameters
    $.ajax({
    url: 'https://api.bex.co.za/api/service/login?username=' + user + '&password=' + pass,
    type: 'POST',
    async: false, //dont run asynchronously
    success: function (data) {
    if (data.ex) { //if the network call succeeds, it may still contain an error (ex)
    alert('The following error occured: ' + data.ex); //display the error
    return;
    };
    alert('Your token is: ' + data.value); //display the valid token
    },
    error: function (ex) { //this will fire for network related issues
        alert(ex.responseText);
        }
    });
</script>
</body>
</html>
```

## Parameters

Parameter | FieldType | Required | Description
--------- | --------- | -------- | -----------
username | String | Yes |Your integration account _username_.
password | String | Yes |Your integration account _password_ (case sensitive).

## Transportation

The service supports GET and POST requests. We recommend POST requests to avoid caching issues.
The service supports URL parameters or JSON parameters in the message body

Example Service Call using the URL Parameters

URL: `http://insight.bex.co.za/api/service/login?username=user123&password=pass1234`

<aside class="notice">
    All URL-based field values must be URL encoded if special characters are used
</aside>

## Response

> Sample json response.

```json
{
    "id": 19696,
 // Here's the token -------------------------------------------------------
    "value": "dOdw98ycsih298749bd6MbZre8o7tt3r3nhfowo8dnBDpEJvkzcQG1vb44sy",
 // ------------------------------------------------------------------------
    "isStrongPassword": "True",
    "isEmailVerified": "True",
    "email": "your.email@company.com",
    "signalREnabled": "False",
    "allowApiLogin": "False"
}
```

The response body is structured as follows:

Attribute | Type | Description
--------- | ---- | -----------
id | int | An internal BEX ID used to identify your user.
value | string | Your unique and private token.
isStrongPassword | string | Confirms whether your password meets our complexity requirements.
isEmailVerified	| string | Your email address is used to recover forgotten passwords or lost tokens and needs to be verified before transactions are possible.
email | string | The recovery email address against which this token is registered.
signalRenabled | string | A feature-flag used internally on our platform.
allowApiLogin | string | A feature-flag used internally to grant additional login rights to your user identity. This is not a requirement to use our API endpoints.


**With your token now generated you can proceed to call our security restricted API’s.**

> Javascript showing the token defined in the headers.

```javascript
var settings = {
  "url": "https://build.bex.co.za/api/service/submitwaybillwia",
  "method": "POST",
  "timeout": 0,
  "headers": {
// -- Example showing the token defined in the http headers
    "token": "6mM7GCuPdaemdvGUATbNmC8GuPuPmdvGgCSg",
// --------------------------------------------------------
    "Content-Type": "application/json"
  }
```

To do so, you *include it as a header attribute* in the HTTP headers of the call you are making to the respective API endpoints. The attribute is to be titled **token**