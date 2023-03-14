# Tracking

## Overview

You can query our courier platform for up-to-the-minute tracking information pertaining to your shipments. We have exposed a lightweight tracking service that returns non-sensitive tracking events for the shipments queried.
To cater for customers wishing to display this tracking information on their own websites we include a schema definition in the tracking response that can be used to bind the fields to a client grid implemented on your website.

## Endpoint
Service Relative URL: `/api/waybillquicktrackingv3customtreeview`

## Transportation

The service allows for the submission of up to ten waybill and/or customer reference numbers per API call. We do not support partial matches nor do we support wildcards.
You are however able to submit a combination of waybill and reference numbers in a single call and the platform will return results for matches, be it a waybill, a reference, or both.

You can submit a **GET** request as follows:

`https://api.bex.co.za/api/waybillquicktrackingv3customtreeview?searchItems=ACP1055352,MDH42240,MA210214`

> Sample json message body

```json
{
	“searchItems”: ”ACP1055352,MDH42240,MA210214”
}
```

## Parameters

Parameter | Type | Required | Description
----------| ---- | -------- | -----------
searchItems | string | Yes | A comma separated list of items to be searched. Can be either a waybill, reference number, or a combination of both.

## Response

> Sample json tracking response

```json
{
    "id": 0,
    "items": [
    // first object is our data-definition object, identified by the groupingIndex == -1.
        {
            "id": 0,
            "dataTypeId": 4,
            "groupingIndex": -1,
            "headerName": "Tracking",
            "col1": "Date",
            "col2": "Waybill",
            "col3": "Senders Ref",
            "col4": "Tracking Description",
            "col5": "User",
            "internalRef": "Internal Ref"
        },
    // subsequent objects are the tracking events themselves, identified by the groupingIndex == 0
        {
            "id": 13751791,
            "dataTypeId": 2,
            "headerName": "Tracking",
            "col1": "06-09-2021 08:03",
            "col2": "ACP1055352",
            "col3": "740493",
            "col4": "Delivered to PRTER ME.A at 08:26 on 03 Sep 2021",
            "col5": "SAMMY GROBLER",
            "internalRef": "13751791",
            "groupingIndex": 0
        },
        {
            "id": 13751791,
            "dataTypeId": 2,
            "headerName": "Tracking",
            "col1": "03-09-2021 08:26",
            "col2": "",
            "col3": "740493",
            "col4": "Driver confirmed successful delivery",
            "col5": "JACKIE MATHOLE",
            "internalRef": "13751791",
            "groupingIndex": 0
        },
        {
            "id": 13751791,
            "dataTypeId": 2,
            "headerName": "Tracking",
            "col1": "03-09-2021 07:06",
            "col2": "",
            "col3": "740493",
            "col4": "Loaded onto vehicle for delivery",
            "col5": "MORAKA RAMOKGOPA",
            "internalRef": "13751791",
            "groupingIndex": 0
        },
        {
            "id": 13751791,
            "dataTypeId": 2,
            "headerName": "Tracking",
            "col1": "03-09-2021 06:37",
            "col2": "",
            "col3": "740493",
            "col4": "Arrived into PRY depot",
            "col5": "MORAKA RAMOKGOPA",
            "internalRef": "13751791",
            "groupingIndex": 0
        },
        {
            "id": 13751791,
            "dataTypeId": 2,
            "headerName": "Tracking",
            "col1": "02-09-2021 21:35",
            "col2": "",
            "col3": "740493",
            "col4": "Departed JNB depot for PRY branch",
            "col5": "GEOFREY SEANEGO",
            "internalRef": "13751791",
            "groupingIndex": 0
        },
        {
            "id": 13751791,
            "dataTypeId": 2,
            "headerName": "Tracking",
            "col1": "02-09-2021 17:49",
            "col2": "",
            "col3": "740493",
            "col4": "Shipment integrated to courier system",
            "col5": "JONATHAN DAVIS",
            "internalRef": "13751791",
            "groupingIndex": 0
        }
    ]
}

```

The response object contains an items array. The first object in this array is our data definition object. It’s purpose is to communicate the bindings of the column names to their column numbers in the subsequent objects. The purpose of this first object is to allow you to use this information for the binding up of the data to a client grid on your website if you wish.

Attribute | Type | Description
--------- | ---- | -----------
id | int | Our internal ID which has no relevance to you.
items |array | An array containing the tracking event objects.


The _objects_ contained in the _items_ array are comprised as follows:

Attribute | Type | Description
--------- | ---- | -----------
id | int | Our internal ID which has no relevance to you.
dataTypeId | int | Internal to BEX and of no use to you.
groupingIndex | int | A tree hierarchy index used to identify parent objects from children.
headerName | string | The name of the entity being returned, in this case “Tracking”.
col1 | string | Column1, typically the DateTime the event occurred.
col2 | string | Column2, typically the Waybill Number of the shipment.
col3 | string | Column3, typically the Customer reference supplied for the shipment.
col4 | string | Column4, typically the description of the Tracking event.
col5 | string |Column5, the BEX member of staff who recorded the event.
internalRef | string | Our internal ID which has no relevance to you.