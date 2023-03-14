# Quoting

## Overview
The BEX quoting service allows you to view dynamic shipping pricing across our range of delivery services. By providing us with your shipment addressing information we can return a pricing spread as well as delivery estimations which you can use to make a more informed shipping choice.

It is important to note that quoting on our platform is a “just-in-time” process and we do not store your quotes (nor return a quote number) for later acceptance by yourself. Instead, our implementation is like that of airline bookings where prices quoted are as at “now”. If you choose not to make a booking the prices may change when next you log onto the platform.

## Endpoint
Service Relative URL: `/api/getquoteservicedeliverypricingV2`



## Parameters

> Sample json quoting message body

```json
{
    "dispatchDate": "2021-09-03",
    "totalWeight": "10",
    "dimMass": "25000",
    "accountNumber": "316085",
    "originSuburb": "Ballito",
    "originPostCode": "4319",
    // If you provide coordinates as well as location names we will favour the coordinates
    "originLatCoord": -29.1234,
    "originLongCoord": "18.5099",
    "destinationLatCoord": "-26.1327",
    "destinationLongCoord": "28.1957"
}
```

To offer an accurate price we require the following parameters from you.

Parameter | Type | Description
--------- | ---- | -----------
dispatchDate | Date | The intended date of shipping.
totalWeight | Decimal | The total actual weight in kilograms (as placed on a scale) of all of the goods to be shipped
dimMass | Decimal | The sum of the total cubic centimetres of all of the goods to be shipped. (length x breadth x height) + (length x breadth x height)
accountNumber | String |Your shipping account number with BEX.
originSuburb | String | The suburb from where we will collect your shipment
originPostCode | String | The postcode of the suburb from where we will collect your shipment.
destinationSuburb | String | The suburb where we will deliver your shipment
destinationPostCode | String | The postcode of the suburb where we will deliver your shipment.
originLatCoord | Decimal | The GPS latitude (north-south position) of your collection location, expressed in decimal notation.
originLonCoord | Decimal | The GPS longitude (east-west position) of your collection location, expressed in decimal notation.
destinationLatCoord | Decimal | The GPS latitude (north-south position) of your delivery location, expressed in decimal notation.
destinationLonCoord | Decimal | The GPS longitude (east-west position) of your delivery location, expressed in decimal notation.

<aside class="notice">
If you provide us the GPS position of the respective collection and/or delivery locations you are not required to provide the Suburb, Town or Postcode information for that location. If you provide us with both (the GPS coordinates as well as the Suburb/Postcode pairs) we will favour and use exclusively the GPS position in the building of your quoted pricing.
</aside>

> We are flexible as to how you supply the addressing.

```json
// Snippet 1: Just coordinates
{
    "originLatCoord":"-26.2462726",
    "originLonCoord":"28.0969053",
    "destinationLatCoord":"-26.5372133",
    "destinationLonCoord":"29.1284535",
    "totalWeight":"43",
    "dimMass":"224000"
}

// Snippet 2: Just location names
{
    "originSuburb":"Steeledale",
    "originPostCode":"2197",
    "destinationSuburb":"Secunda",
    "destinationPostCode":"2302",
    "totalWeight":"43",
    "dimMass":"224000"
}

// Snippet 3: A combination of both
{
    "originLatCoord":"-26.2462726",
    "originLonCoord":"28.0969053",
    "destinationSuburb":"Secunda",
    "destinationPostCode":"2302",
    "totalWeight":"43",
    "dimMass":"224000"
}
```

**Calculation of the delivery charges**

In courier we use the greater of the _actual_ weight or the _volume_ of the goods to be shipped. Basically, we charge by weight OR by the physical size of the goods, whichever is greater (aircraft get expensive when flying empty boxes around).

These 2 parameters are:

1. `totalWeight` The total weight in kilograms of **all** the goods on this order
1. `dimMass` The sum of the total cubic centimetres of **all** the goods on this order

**Example:**
A customer order is going from Sandton to Secunda. The order is made up of **2 boxes** as follows:

1. A stock 1 box (40cm x 40cm x 20cm) and weighing 18kgs
1. A stock 4 box (80cm x 80cm x 30cm) and weighing 25kgs.

Looking at the code snippets on the right, you can see that the quotation request is to be structured as follows:

* `"totalWeight": "43"`
* `"dimMass": "224000"` (40x40x20) + (80x80x30) = 224,000

For the addressing, first prize we would like the co-ordinates of the collection and delivery locations (json snippet 1), otherwise you can pass us the suburb and postcode of the collection and delivery locations (snippet 2 or 3). 

## Response

> json quoting response.

```json
{
    "id": 0,
    "items": [
        {
            "id": 0,
            "customerName": "BOEING AIRCRAFT COMPANY",
            "serviceId": 449,
            "serviceCodeStr": "ONC",
            "serviceNameStr": "OVERNIGHT COURIER",
            "originSuburb": "AGGENEYS",
            "originCity": "AGGENEYS",
            "originRateCategory": "REMOTE",
            "originBranchStr": "CAPE TOWN",
            "originHub": "CPT",
            "originLocationCode": "",
            "destinationSuburb": "CROYDON",
            "destinationCity": "KEMPTON PARK",
            "destinationRateCategory": "MAJOR",
            "destinationBranchStr": "JOHANNESBURG",
            "destinationHub": "JNB",
            "destinationLocationCode": "",
            "estimatedTransitDays": 4,
            "scheduledTransitDays": 4,
            "estimatedHolidays": 0,
            "scheduledHolidays": 0,
            "scheduleBy": "2021-10-05 16:00:00",
            "estimatedCollectionDateAndTime": "2021-10-05 18:00:00",
            "latestCollectionDateAndTime": "2021-10-05 18:00:00",
            "estimatedDeliveryDateAndTime": "2021-10-09 10:00:00",
            "scheduledDeliveryDateAndTime": "2021-10-09 14:00:00",
            "isDeliveryGuaranteed": "True",
            "surchargeAddons": "SURSAT",
            "customerChargeableWeight": 10,
            "baseRateTotal": 0.00,
            "fuel": 0.00,
            "insuranceRequired": 0.00,
            "otherSurcharges": 0.00,
            "surchargeTotal": 0.00,
            "subTotal": 0.00,
            "vat": 0.00,
            "totalCharges": 0.00
        },
        {
            "id": 0,
            "customerName": "BOEING AIRCRAFT COMPANY",
            "serviceId": 449,
            "serviceCodeStr": "ONC",
            "serviceNameStr": "OVERNIGHT COURIER",
            "originSuburb": "AGGENEYS",
            "originCity": "AGGENEYS",
            "originRateCategory": "REMOTE",
            "originBranchStr": "CAPE TOWN",
            "originHub": "CPT",
            "originLocationCode": "",
            "destinationSuburb": "CROYDON",
            "destinationCity": "KEMPTON PARK",
            "destinationRateCategory": "MAJOR",
            "destinationBranchStr": "JOHANNESBURG",
            "destinationHub": "JNB",
            "destinationLocationCode": "",
            "estimatedTransitDays": 4,
            "scheduledTransitDays": 4,
            "estimatedHolidays": 1,
            "scheduledHolidays": 1,
            "scheduleBy": "2021-10-03 16:00:00",
            "estimatedCollectionDateAndTime": "2021-10-03 18:00:00",
            "latestCollectionDateAndTime": "2021-10-03 18:00:00",
            "estimatedDeliveryDateAndTime": "2021-10-08 10:30:00",
            "scheduledDeliveryDateAndTime": "2021-10-08 11:00:00",
            "isDeliveryGuaranteed": "True",
            "surchargeAddons": "",
            "customerChargeableWeight": 10,
            "baseRateTotal": 0.00,
            "fuel": 0.00,
            "insuranceRequired": 0.00,
            "otherSurcharges": 0.00,
            "surchargeTotal": 0.00,
            "subTotal": 0.00,
            "vat": 0.00,
            "totalCharges": 0.00
        }
    ]
}

```
In the **response object** you will find an **items** _array of objects_ containing the different service and pricing options available for the shipment routing you have requested.

The objects in the items array are comprised as follows:

Attribute | Type | Description
--------- | ---- | -----------
Id | Int | An internal BEX field.
customerName | String | Your account name with BEX.
serviced | Int | An internal id used to identify the shipping service in question.
serviceCodeStr | String | The shipping service code.
serviceNameStr | String | The full name of the shipping service.
originSuburb | String | The suburb we calculated from where your shipment will be sent.
originCity | String | The city from where your shipment will be sent.
originRateCategory | String | The billing category of the location from where your shipment will be sent. 
originBranchStr | String | The name of the BEX branch who will handle the collection of your shipment.
originHub | String | The 3 letter code of the BEX branch that will collect your shipment.
originLocationCode | String | The 3 letter town code of the location from where your shipment will be sent.
destinationSuburb | String | The suburb where your delivery will take place.
destinationCity | String | The city where your delivery will take place.
destinationRateCategory | String | The billing category of the location where your shipment will be delivered.
destinationBranchStr | String | The name of the BEX branch that will handle the delivery of your shipment.
destinationHub | String | The 3 letter code of the BEX branch that will deliver your shipment.
destinationLocationCode | String | The 3 letter town code of the location where your shipment will be delivered.
estimatedTransitDays | Int | The number of business days we believe it will take to complete delivery of your shipment.
scheduledTransitDays | Int | The number of business days we may take to complete the delivery of your shipment, based on the service level of the shipping service.
estimatedHolidays | Int | The number of non-working days falling within the period of our delivery lead-time estimation.
scheduledHolidays | Int | The number of non-working days falling within the period of our service sla lead-time.
scheduleBy | Datetime | The cut-off time by which you need to capture your shipping request into our system to ensure the delivery lead times are achievable.
estimatedCollectionDateAndTime | Datetime | The estimated date and time we can collect your shipment at its origin location to commence the shipping process.
latestCollectionDateAndTime | DateTime | Our operational cut-off time after which we are unable to collect your shipment.
estimatedDeliveryDateAndTime | DateTime | Our estimated date and time of delivery of your shipment.
scheduledDeliveryDateAndTime | DateTime | The scheduled date and time of delivery of your shipment.
isDeliveryGuaranteed | String | Some delivery schedules are guaranteed, depending on the shipping service selected.
surchargeAddons | String | An optional service-addon which if selected, will provide additional options such as a Saturday or After hours delivery.