# View voucher or confirmation status

The object returned by the [/booking/book](../../../../openapi/reference/operation/bookingBook) service contains booking details that can be used to display an order summary to the customer.

Confirmed bookings of freesale products will return a `voucherKey` and `voucherURL`. The `voucherURL` is accessible by the customer to view their voucher.

Pending bookings of on-request products will return `null` in the `voucherKey`/`voucherURL` fields. These fields will only contain values once the booking is confirmed.

A `bookingStatus` object is included in the response that contains the status of the booking.

```javascript
"bookingStatus": {
  "status": 1,
  "text": "Paid &amp; Confirmed",
  "type": "CONFIRMED",
  "level": "ITEM",
  "failed": false,
  "confirmed": true,
  "amended": false,
  "pending": false,
  "cancelled": false
}
```

The status field corresponds to a number described in [bookingStatus field values and meanings](../../appendices/#bookingStatus-field-values-and-meanings).

Boolean flags that you can inspect to determine the booking status of the item:
- `failed`
- `cancelled`
- `confirmed`
- `amended`
- `pending`

## Pending bookings
A booking is considered *pending* if the booking process is 'in progress'. For example, an on-request booking would be *pending* until it is confirmed/rejected by the supplier.

If a customer has made an amendment to an on-request booking that is yet to be accepted by the supplier, the booking would then have a status of *amended* when the supplier or customer service accepts the amendment.

**Example response object:**

```javascript
{
  "errorReference": null,
  "data": {
    "sortOrder": 0,
    "rulesApplied": [],
    "bookingStatus": {
      "status": 3,
      "text": "Confirmed",
      "type": "CONFIRMED",
      "level": "ITINERARY",
      "confirmed": true,
      "pending": false,
      "amended": false,
      "cancelled": false,
      "failed": false
    },
    "itemSummaries": [{
      "sortOrder": 0,
      "rulesApplied": [],
      "bookingStatus": {
        "status": 1,
        "text": "Paid &amp; Confirmed",
        "type": "CONFIRMED",
        "level": "ITEM",
        "pending": false,
        "failed": false,
        "confirmed": true,
        "amended": false,
        "cancelled": false
      },
      "travellerAgeBands": [{
        "sortOrder": 0,
        "count": 1,
        "pluralDescription": "Adults",
        "description": "Adult",
        "ageBandId": 1
      }],
      "voucherKey": "1005851866:4af44c13ecf3f1a7d3f9ef2fc00c2257e08fa42ae20f877f3039ff9b52aba24e:580669678",
      "voucherURL": "https://viatorapi.live.rc.viator.com/service/merchant/voucher.jspa?code=1005851866:4af44c13ecf3f1a7d3f9ef2fc00c2257e08fa42ae20f877f3039ff9b52aba24e:580669678&embedResources=false",
      "voucherRequirements": "You must present a paper voucher for this to",
      "productPulledDown": false,
      "merchantCancellable": true,
      "productWidgetList": null,
      "savingAmount": 0,
      "passbooks": null,
      "termsAndConditions": null,
      "itineraryId": 1000024753,
      "tourGradeCode": "1DAYBOAT",
      "productCode": "2065CPT",
      "leadTravellerSurname": "Test",
      "distributorItemRef": "distroItemRefJDP1803151135",
      "languageServicesLanguageCode": "en",
      "travelDate": "2016-02-01",
      "price": 3.72,
      "bookingEngineId": "UF",
      "merchantNetPrice": 3.51,
      "merchantNetPriceFormatted": "$3.51",
      "leadTravellerTitle": "Mr",
      "leadTravellerFirstname": "Homer",
      "lastRetailPrice": 3.51,
      "destId": 318,
      "voucherOption": "VOUCHER_PAPER_ONLY",
      "productTitle": "Cape Town City Hop-on Hop-off Tour",
      "itemId": 700025496,
      "barcodeOption": "perperson",
      "barcodeType": "code128",
      "obfsId": 3696,
      "priceFormatted": "$3.72",
      "savingAmountFormated": "",
      "priceUSD": 3.72,
      "lastRetailPriceFormatted": "$3.51",
      "departsFrom": "Cape Town, South Africa",
      "hoursConfirmed": 0,
      "currencyCode": "USD"
    }],
    "voucherKey": "1005851866:4af44c13ecf3f1a7d3f9ef2fc00c2257e08fa42ae20f877f3039ff9b52aba24e:580669678",
    "voucherURL": "https://viatorapi.live.rc.viator.com/service/merchant/voucher.jspa?code=1005851866:4af44c13ecf3f1a7d3f9ef2fc00c2257e08fa42ae20f877f3039ff9b52aba24e:580669678&embedResources=false",
    "bookerEmail": "apitest@viator.com",
    "userId": null,
    "itineraryId": 1000024753,
    "exchangeRate": 1,
    "totalPriceFormatted": "$3.51",
    "totalPriceUSD": 3.51,
    "bookingDate": "2015-03-17",
    "distributorRef": "distroRefJDP1803151135",
    "totalPrice": 3.51,
    "hasVoucher": true,
    "currencyCode": "USD"
  },
  "dateStamp": "2015-03-17T17:36:32+0000",
  "errorType": null,
  "errorMessage": null,
  "errorName": null,
  "success": true,
  "totalCount": 1,
  "vmid": "321003",
  "errorMessageText": null
}
```

## Checking the status of bookings

### Checking the status of a single booking:

- [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) can be used to check the status of a booking after it has been purchased. This is useful for checking the status of a pending booking, particularly if there are multiple items within the booking.

  It is recommended that you poll the service no more than once per hour.

### Checking the status of multiple bookings:

- [/booking/status](../../../../openapi/reference/operation/bookingStatus) will return the status of all bookings, based on the following:
  - booking date range
  - travel date range
  - specific distributor/distributor item references
  - lead traveler date/first name

This is useful for checking the status of recently-made, but still *pending* bookings, or those that will commence soon.

**Note:** You can only poll this service (successful calls) once every 30 minutes.

## Confirming the booking via email

Upon receiving a successful response from [/booking/book](../../../../openapi/reference/operation/bookingBook), merchant partners should confirm the booking with the customer via email. This email must be sent by the merchant partner (not Viator).

### Sample confirmation email

<pre>
Dear Traveller,

Thank you for booking with [PARTNER NAME] on [www.domain.com]. Your booking is confirmed. This is your booking notification and receipt. Please retain this email for your records.

Please note:

We offer three types of vouchers. Be sure to check below for what type of voucher your tour / activity requires. Voucher requirements vary by tour, so if you've booked more than one tour, be sure to check each one. See below for instructions.

HOTEL/FLIGHT ITINERARY
-----------------------------------------------------------------------
[Your standard reply here]

TOURS
-----------------------------------------------------------------------

1. Name: Whale Watching Cruise
  e-Voucher or Paper Voucher Accepted
  You can present either a paper or an electronic voucher for this activity.
  Voucher: [VOUCHERURL]
  Please click on the above link, follow the directions, and print your voucher to present as per instructions on the voucher under 'Important Information'.
2. Name: Boston City Pass
  Paper Voucher Required
  You must present a paper voucher for this tour. Voucher: [VOUCHERURL]
  Please click on the above link, follow the directions, and print your voucher to present as per instructions on the voucher under 'Important Information'.
3. Name: SuperShuttle Airport Transfer
  Voucher Not Required
  You must present a paper voucher for this tour. Voucher: [VOUCHERURL]
  You can present a paper or electronic voucher for this activity, or you can simply present the Lead Traveller’s Photo ID. Our supplier has your reservation on file and only requires proof of identity on the day of travel.

IMPORTANT INFORMATION
-----------------------------------------------------------------------
[Your standard reply here]

TERMS AND CONDITIONS
-----------------------------------------------------------------------
[Your standard reply here]
</pre>