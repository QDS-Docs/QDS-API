# Webhooks Guide

## Getting Started
[//]: # "Confirm this is the email address used for webhook api requests"
In order to use our WebHooks, you need to ensure that you have a valid and actively trading customer account registered with BEX. Once you have your account number, you will need to request a WebHook API key from our <a href="mailto:it@bex.co.za?subject=WebHook%20API%20key%20Request">IT department</a>. We will communicate this to you but it is your responsibility to ensure the secrecy of it.

Before we get started, please ensure that you understand and have implemented the necessary methods required to work with WebHooks. 

## WebHook Registration
The registration is a two-part process that requires two individual requests. 

* First, you need to register your WebHook with us by sending a POST request to our WebHook endpoint located at:

           https://webhook.bex.co.za/api/webhooks/registrations 


> In the header of your request, you should add a new key pair that represents your WebHook API key: 

```json
{ 
    “X-InsightWebHooks-Auth”: “Your API Key” 
}
```

> The request body is made up of:

```json
{
    WebHookUri: “your endpoint where the webhook will post”, 
    Secret: “A secret key that you generate”, 
    Description: “A description for your webhook”, 
    Filters: [ “filterA”, “filterB” ]
}
```

* After you have made this request and you receive a 200 OK response, you need to send another request to:

          https://webhook.bex.co.za/api/ActivateWebHook 

> Providing the request body with the following values:

```json
{
    webHookKey: “your API key”
}
```

<aside class="notice">
Once you have made this request and you receive a 200 OK response, your WebHook is activated and should start receiving information momentarily.
</aside>

## A Word on Filters
We provide several different types of WebHook events and you can subscribe to one or all of them. If you want to subscribe to ALL events, then you should ensure that your code can differentiate between these types of events. In this case, you should provide an empty value for the Filters parameter i.e., Filters: []

If you would like to subscribe to certain events only, then you need to provide each event type you want to subscribe to, e.g. Filters: [ “event 1”, “event 2”, … ]

We provide the following filters/event types:

* pod
* attempted_delivery
* waybill_update
* waybill_note
* tracking_event

<aside class="notice">
More detail on each event type and what payload they contain can be found later in this document.
</aside>

## Unsubscribing

To unsubscribe from our WebHook, send a POST request to:
          https://webhook.bex.co.za/api/UnsubscribeWebHook


> The request body should consist of:

```json
{ 
    webHookKey: “your API key”
}
```

## Filters/Event Types

<aside class="notice">
When parsing the payload that our WebHook sends you, we will provide you with the requested event types or you can elect to receive all of them.
</aside> 

## Basic Payload Structure

> The basic payload structure for our WebHook would look like this:

```json
{ 
    “Id”: “a unique id for this message”, 
    “Attempt”: 1, 
    “Properties”: { }, 
    “Notifications”: [{
        “Action”: “tracking_type”, 
        …
    }]
}
```

As part of the request to your WebHook endpoint, we also include a special header:


* Header Key: ms-signature 
* Header Value: sha256=calculatedHashValueGoesHere


## Notifications Property
The notifications property will contain the type of notification you are receiving (“Action”) followed by the list of properties relating to that specific event type. These properties are listed below for your convenience.

## Verifying WebHook Signature
Since we are only sending you a request with a payload and that we have no knowledge of your infrastructure, the message will be sent “anonymously” and won’t provide any mechanism to allow authentication to a closed system. It is your responsibility to ensure that the endpoint you specify accepts anonymous users and that you perform message validation as an alternative to authentication.

Every WebHook request that we send will have the special ms-signature header key containing the hash of the body we’ve sent. You should calculate the signature using the supplied secret (during registration) based on the body of the message you’ve received from us.

> A C# example of calculating the signature would be:

```csharp
var secret = Encoding.UTF8.GetBytes(secretYouProvidedDuringRegistration);
using (var hasher = new HMACSHA256(secret)) 
{
    var serializedBody = body.ToString();
    request.Content = new StringContent(serializedBody, Encoding.UTF8, “application/json”);
    
    var data = Encoding.UTF8.GetBytes(serializedBody);
    var sha256 = hasher.ComputeHash(data);
    var headerValue = EncodingUtilities.ToHex(sha256);
}
```

## Event Types/Payloads
Each event will be sent using the basic payload structure and contain different properties for each type of event.


>**Tracking Events**

```json
{
    “Action”: “tracking_event”, 
    “WaybillNumber”: “12345”, 
    “TrackingDate”: “2021-07-12 11:59:25”, 
    “TrackingUser”: “John Doe”, 
    “TrackingDescription”: “tracking description”, 
    “TrackingReference”: “some reference”, 
    “TrackingTypeId”: -1
}
```



>**POD**

```json
{
    “Action”: “tracking_event”, 
    “WaybillNumber”: “12345”, 
    “TrackingDate”: “2021-07-12 11:59:25”, 
    “TrackingUser”: “John Doe”, 
    “TrackingDescription”: “tracking description”, 
    “TrackingReference”: “some reference”, 
    “TrackingTypeId”: -1
}	{
    “Action”: “pod”, 
    “WaybillNumber”: “12345”, 
    “PODDate”: “2021-07-12”, 
    “PODTime”: “11:34:25”, 
    “Signee”: “Jane Doe”, 
    “PODPieces”: 1
}
```

>**Attempted Delivery**

```json
{
    “Action”: “attempted_delivery”, 
    “WaybillNumber”: “12345”, 
    “AttemptedDate”: “2021-07-12”, 
    “AttemptedTime”: “11:12:16”, 
    “NonDeliveryReason”: “Wrong route”, 
    “DriverComment”: “driver”
}
```

>**Waybill Note**

```json
{
    “Action”: “waybill_note”, 
    “PostedBy”: “John Doe”, 
    “NoteDate”: “2021-07-12 12:08:16”, 
    “NoteText”: “Customer will phone when home”
}
```
>**Waybill Update**

```json
{
    “Action”: “waybill_update”, 
    “WaybillNumber”: “12345”, 
    “Service”: “ONC”, 
    “InsuranceRequired”: false, 
    “InsuranceValue”: 0, 
    “ActualWeight”: 1.0, 
    “VolumetricWeight”: 1.0, 
    “ChargeableWeight”: 1.0, 
    “BaseRateTotal”: 25, 
    “SurchargesTotal”: 17.50, 
    “SubTotal”: 42.50, 
    “Vat”: 6.38,
    “TotalCharges”: 48.88, 
    “Parcels”: [
    { 
          “ParcelBarcodeNumber”: “P1”, 
          “Dimension1”: 40, 
          “Dimension2”: 30, 
          “Dimension3”: 1, 
          “Weight”: 1.0, 
          “ParcelDescription”: “”
    }, …
    ]
}	
```

<br>
<br>

*Tracking Event ID Descriptions:*

Tracking Type ID | Description
-------- | -----------
1 |	Shipment handed to courier.
4 |	Shipment departed XYZ branch/depot.
5 |	Shipment arrived at XYZ branch/depot.
7 |	Shipment loaded onto vehicle for delivery.
9 |	Delivery attempt failed because (reason variable)
13 | Shipment integrated into our system.

<aside class="notice">
To subscribe to any of these events, you can either leave the Filters parameter empty during initial WebHook registration to receive all events, or you can specify the events you want by using the values as shown in the Action parameter of the relevant event type i.e., tracking_event, pod, attempted_delivery, waybill_note, waybill_update.
</aside>