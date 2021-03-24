# Availability services

Product availability information can be retrieved with the following services:

- [/booking/availability](../../../../openapi/reference/operation/bookingAvailability): get the tourgrade with the lowest price available on a day
- [/booking/availability/dates](../../../../openapi/reference/operation/bookingAvailabilityDates): get all available dates for a product excluding days it does not operate and blockouts
- [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades): list all available tour grades for a specific day
- [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix): returns available days only (ie days which have at least one tour grade available), and the pricing matrix for that tour grade on that day

## Example: multiple departures in a single day

Multiple departures in a single day (each represented by a tour grade) and the language options (langServices).

This request is for 3 adults on a helicopter tour:
- **Note:** No prices are returned if the tour grade is unavailable.

**Request object** ([/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades)):

```javascript
{
  "productCode": "2280AAHT",
  "bookingDate": "2013-05-11",
  "currencyCode": "EUR",
  "ageBands": [{
    "bandId": 1,
    "count": 3
  }]
}
```

**Response object** ([/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades))
:

```javascript
{
  "data": [{
    "available": false,
    "ageBands": null,
    "langServices": null,
    "gradeCode": "EARLYM",
    "unavailableReason": "BOOKING_CUTOFF_EXPIRED",
    "gradeTitle": "Early Morning Departure",
    "gradeDepartureTime": "",
    "gradeDescription": "Flight departs Las Vegas between 7am & 8am",
    "defaultLanguageCode": "en",
    "ageBandsRequired": null,
    "currencyCode": "ERROR",
    "retailPrice": 0,
    "bookingDate": "2013-05-11",
    "retailPriceFormatted": "",
    "merchantNetPrice": 0,
    "merchantNetPriceFormatted": "",
    "sortOrder": 1
  },
  {
    "available": false,
    "ageBands": null,
    "langServices": null,
    "gradeCode": "LATEM",
    "unavailableReason": "BOOKING_CUTOFF_EXPIRED",
    "gradeTitle": "Late Morning Departure",
    "gradeDepartureTime": "",
    "gradeDescription": "Flight departs Las Vegas between 9:45am & 10:45am",
    "defaultLanguageCode": "en",
    "ageBandsRequired": null,
    "currencyCode": "ERROR",
    "retailPrice": 0,
    "bookingDate": "2013-05-11",
    "retailPriceFormatted": "",
    "merchantNetPrice": 0,
    "merchantNetPriceFormatted": "",
    "sortOrder": 2
  },
  {
    "available": false,
    "ageBands": null,
    "langServices": null,
    "gradeCode": "EARLYA",
    "unavailableReason": "BOOKING_CUTOFF_EXPIRED",
    "gradeTitle": "Early Afternoon Departure",
    "gradeDepartureTime": "2:50 PM",
    "gradeDescription": "Flight departs Las Vegas between 12:30pm & 1:30pm",
    "defaultLanguageCode": "en",
    "ageBandsRequired": null,
    "currencyCode": "ERROR",
    "retailPrice": 0,
    "bookingDate": "2013-05-11",
    "retailPriceFormatted": "",
    "merchantNetPrice": 0,
    "merchantNetPriceFormatted": "",
    "sortOrder": 3
  },
  {
    "available": false,
    "ageBands": null,
    "langServices": null,
    "gradeCode": "LATEA",
    "unavailableReason": "BOOKING_CUTOFF_EXPIRED",
    "gradeTitle": "Late Afternoon Departure",
    "gradeDepartureTime": "",
    "gradeDescription": "Flight departs Las Vegas between 3:15pm & 4:15pm; available Ap",
    "defaultLanguageCode": "en",
    "ageBandsRequired": null,
    "currencyCode": "ERROR",
    "retailPrice": 0,
    "bookingDate": "2013-05-11",
    "retailPriceFormatted": "",
    "merchantNetPrice": 0,
    "merchantNetPriceFormatted": "",
    "sortOrder": 4
  }],
  "vmid": "221001",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2013-06-04T17:01:34+0000",
  "totalCount": 1,
  "errorReference": null,
  "errorMessageText": null,
  "success": true,
  "errorName": null
}
```

## Example: traveler mix mismatch

The request is for five adults, but this product only support up to four adults.

**Example request object** ([/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades)):
```javascript
{
  "productCode": "2280ULTWED",
  "bookingDate": "2013-12-11",
  "currencyCode": "EUR",
  "ageBands": [{
    "bandId": 1,
    "count": 5
  }]
}
```

**Example response object**
The response contains `"TRAVELLER_MISMATCH"` and you can see the `ageBandsRequired` values for the adult (1) age band in the available tour grade.

```javascript
{
  "data": [{
    "available": false,
    "ageBands": null,
    "langServices": null,
    "gradeCode": "DEFAULT",
    "unavailableReason": "TRAVELLER_MISMATCH",
    "gradeTitle": "DEFAULT",
    "gradeDepartureTime": "12:00 AM",
    "gradeDescription": "DEFAULT",
    "defaultLanguageCode": "en",
    "ageBandsRequired": [
    [{
      "bandId": 1,
      "minimumCountRequired": 2,
      "maximumCountRequired": 2
    }],
    [{
      "bandId": 1,
      "minimumCountRequired": 3,
      "maximumCountRequired": 3
    }],
    [{
      "bandId": 1,
      "minimumCountRequired": 4,
      "maximumCountRequired": 4
    }]],
    "currencyCode": "ERROR",
    "retailPrice": 0,
    "bookingDate": "2013-12-11",
    "retailPriceFormatted": "",
    "merchantNetPrice": 0,
    "merchantNetPriceFormatted": "",
    "sortOrder": 1
  }],
  "vmid": "221001",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2013-06-04T17:02:35+0000",
  "totalCount": 1,
  "errorReference": null,
  "errorMessageText": null,
  "success": true,
  "errorName": null
}
```
