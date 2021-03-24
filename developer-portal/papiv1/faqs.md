# FAQs

## Is a confirmation email sent to the customer or supplier when a booking is made in the sandbox environment?

* No.

## How do I make a demo booking?

* To make a demo booking, make sure you set `demo` to `true` in your request to the [/booking/book](../../../openapi/reference/operation/bookingBook) service.

## What should I do if I successfully create a test booking in the Live environment?

* Firstly, please don't make test bookings in the Live environment, as doing so may cause a confirmation email to be sent to the product supplier. Nonetheless, if you have made a test booking, cancel it using the [/bookings/{id}/cancel](../../../openapi/reference/operation/cancelBooking) service; or, [send an email to dpsupport@viator.com](mailto:dpsupport@viator.com) and include both "CANCEL" and the booking reference number in the subject line.

## Which currencies can I use when making a booking?

* You can make a booking using the [/booking/book](../../../openapi/reference/operation/bookingBook) endpoint using any of the [supported currencies](../appendices/#supported-currency-codes).
* If you attempt to use a non-supported currency, you will receive an error message similar to the following:

```json
{
  "errorReference": "3D45567E:2D4A_0A5D033A:01BB_5F616D10_1FBBC9:7035",
  "data": null,
  "dateStamp": "2020-09-15T18:41:00+0000",
  "errorType": "EXCEPTION",
  "errorCodes": [
    "UNKNOWN_ERROR"
  ],
  "errorMessage": [
    "Merchant API does not allow the specified currency"
  ],
  "errorName": "RuntimeException",
  "extraInfo": {},
  "extraObject": null,
  "success": false,
  "totalCount": 1,
  "errorMessageText": [
    "Merchant API does not allow the specified currency"
  ],
  "vmid": "331001"
}
```

## Why am I having an SSL handshake issue?

* It may be that your SSL certificates have expired. Check this. Also, if you are using Java, make sure that it's [updated to the latest version](https://www.java.com/en/download/).

## What is the difference between `merchantNetPrice` and `price` in the response from the [/search/products](../../../openapi/reference/operation/searchProducts) service?

* `merchantNetPrice` is the amount you, as the merchant partner, will be invoiced for, excluding any fees.
* `price` is the price at which Viator sells the product

## Why is a different price displayed in [/booking/availability/tourgrades](../../../openapi/reference/operation/bookingAvailabilityTourgrades) and [/product](../../../openapi/reference/operation/product)?

* [/product](../../../openapi/reference/operation/product) displays the lowest possible price per traveller; whereas, [/booking/availability/tourgrades](../../../openapi/reference/operation/bookingAvailabilityTourgrades) shows the per-traveller price based on the age-band and number of travellers within that age-band.

## How should I handle the case where a destination is missing its latitude, longitude or time zone?

* Some destinations in the **sandbox** environment may be missing geolocation or time zone data. However, if you encounter a destination in the **Live** environment with missing information, this can be regarded as an unintended omission – please contact us so that we can update our destination information.

## How do I make a test booking?

To make a test booking, make sure you:

- set `firstname` and/or `surname` in the `booker` object to `'test'`.
- set `demo` to `true`

Example:

```javascript
{
  "demo": true,
  "currencyCode": "USD",
  "partnerDetail": {
    "distributorRef": "distroRef0412141435"
  },
  "booker": {
    "email": "apitest@viator.com",
    "firstname": "Test",
    "surname": "Test",
    "title": "Mr"
  },
  "items": [{
    "partnerItemDetail": {
      "distributorItemRef": "distroItemRef0412141435_1"
    },
    "hotelId": null,
    "pickupPoint": null,
    "travelDate": "2015-03-31",
    "productCode": "2916ROME",
    "tourGradeCode": "24HR",
    "languageOptionCode": "en/SERVICE_GUIDE",
    "bookingQuestionAnswers": [{
      "questionId": 100,
      "answer": "120 kgs"
    }],
    "specialRequirements": "",
    "travellers": [{
      "bandId": 1,
      "firstname": "Homer",
      "surname": "Simpson Test",
      "title": "Mr",
      "leadTraveller": true
    }]
  }]
}
```

## Is it possible to use a custom value for `distributorRef` and `distributorItemRef`?

* Yes! In fact, this is what you're supposed to do. You can pass anything you like in these fields; however, if you use a `distributorRef` that has already been used, the API will return the previous booking made with that `distributorRef` rather than creating a new booking.
* **Note**: `distributorRef` must be fewer than 40 characters

## What are some common reasons for bookings to fail in the Viator API?

- `homePhone` or any phone field in the booking request contains spaces. The only acceptable non-numeric characters are: “-“,  “+” , “(“,  and “)”

- `distributorRef` has been re-used.  When making a booking request, a `distributorRef` and `distributorItemRef` must be provided. This is the partner’s ID for the booking, and it must be unique. If a `distributorRef` has been re-used, a booking will not be made and instead, the existing booking will be returned in the response.

- `distributorRef` exceeds 40 characters

- No traveller or an incorrect traveller has been set as the lead traveller in the booking request;
  + `leadTraveller`:`true` must be set for one traveller
  + the `leadTraveller` must be from an `ageBand` that has `treatAsAdult` set to `true`. The data is available in the `ageBands` object in the product details service.

- `languageOptionCode` is invalid
  + To find the valid language option codes for a particular product, have a look at the `langServices` object in the response from the [/product](../../../openapi/reference/operation/product) service; e.g.,

```javascript
"langServices": {
  "en/SERVICE_AUDIO": "English - Audio"
}
```
  + You must then ensure that the `languageOptionCode` in the request to the [/booking/book](../../../openapi/reference/operation/bookingBook) service is populated in the same way; i.e.,

```javascript
"languageOptionCode": "en/SERVICE_AUDIO"
```

## What does it mean if I receive a "Section level access denied" error message?

- This means that your API-key does not have the correct permissions to access the particular service you were attempting to access when you received this error message. If you feel you would like to use this service nonetheless in your implementation, please contact your account manager to discuss having access granted.

## What does it mean if I receive a "503 Service Unavailable" response?

- This means that there was a temporary service outage on our end at that time. We recommend that you re-try the operation until you no longer receive this error.

## Does API rate limiting apply to all services?

- Yes, it does. Regardless of the service you are making requests to, the fundamental rate limits apply equally.

## Can I cancel multiple bookings or items at the same time using the Viator API?

- No, you may only cancel one booking at a time.

## How many concurrent requests can be made of the API from the same IP address?

- Three.

## Will my API-key expire?

- If any API-key is not used for a period of six months, that key is automatically deactivated as a security measure. If this has happened to you, contact your account manager to have the key reactivated or a new key issued to you.

## Why am I getting an empty response when checking booking details?

- If you are attempting to check a booking using the [/booking/status](../../../openapi/reference/operation/bookingStatus) or [/booking/status/items](../../../openapi/reference/operation/bookingStatusItems) endpoints and receive an empty response, it may be that the booking was made with the demo parameter set to `true`, as the booking status endpoints will ignore demo bookings. Please try making the booking again, ensuring the demo parameter is set to `false`. If this also fails, please email [dpsupport@viator.com](mailto:dpsupport@viator.com) and include a copy of the request and response JSON objects.

## Must I always provide details for all travelers when booking a product where `allTravellerNamesRequired`=`true`?

- Approximately 45% of the products in our catalog require all traveller details to be supplied at the time of booking, and this requirement is enforced by the API. While it is technically possible to bypass this requirement – for example, by setting the lead traveler's name, but using dummy values for the the remaining travelers' details ('traveler 2', etc.) – we strongly advise against this, as it can cause problems for suppliers for whom this is a strict requirement. Examples include: Alcatraz tickets, theme park tickets that require a QR code, bookings for flights needing to meet TSA requirements; or, vehicle, Segway or jet-ski rentals. If you are unable to implement the collection of all traveler details on your site, we recommend filtering-out products where this is a strict requirement. You may also request that we filter-out these products for you so that they do not appear in product search results.

## Must I always provide answers to the required booking questions when making a booking?

- Yes. You must provide answers to all necessary booking questions when making a booking. Approximately 40% of the products in our catalog have booking questions, some of which may be necessary to fulfil the suplier's legal requirements. In the case that the customer cannot provide specific details at the time of booking – e.g., a departure flight number – they may enter 'unknown' and the supplier will manually send a follow-up message to ask for this information.

## Why is there such a big difference in price between that given in the product endpoints and the actual price at the time of booking?

- The price returned in the product endpoints is the 'From Price', which is the <u>lowest possible price</u> for an adult passenger when taking into account all available tour grades, group bookings and so forth. The exact price can only be determined when you check the availability for a specific date and passenger/traveler mix. We recommend using the [/booking/availability/tourgrades](../../../openapi/reference/operation/bookingAvailabilityTourgrades) endpoint for this purpose.

## Why doesn't the [/taxonomy/destinations](../../../openapi/reference/operation/taxonomyDestinations) endpoint return continent-level information?

- Products are categorized according to their 'destination', which includes cities, regions and countries. The next logical grouping would be 'by continent'; however, this would be too broad a grouping, resulting in too many search results and lengthy response times if the product catalogue were to be searched by continent. For more information, see: [Categorization of content](../key-concepts/categorization-of-content)

## Will there ever be a discrepancy between the amount charged for a tour and the amount refunded due to currency exchange-rate fluctuations?

- In short: no. Firstly, the cost of the booking and the refund amount are always calculated in the product supplier's native currency – no exchange rate calculations are applied. I.e., if the cost of the booking was USD 100 and the refund percentage is 100% (full refund, as per the response from [/bookings/{booking-reference}/cancel-quote](../../../openapi/reference/operation/cancelBookingQuote)), Viator will simply not invoice you for that USD 100 that we would have if the booking was not canceled. Furthermore, we do not invoice you for the cost of a booking prior to its departure date. 