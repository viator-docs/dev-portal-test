# Checking bookings

The [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) service gets a user's future bookings; i.e., those with travel dates in the future. This service can also be used to check the status of a booking.

## Key concepts
### Sample URL parameters

- `"sessionId=xxx"`, or
- `"voucherKey=xxx"`, or
- `"email=terry.smith@viator.com&lastCCFourDigits=4242"`, or
- `"email=terry.smith@viator.com&itineraryOrItemId=3299307"`

Provide *one* of:
- a `sessionId` for all bookings related to a user account, or
- a `voucherKey`, or
- an email address (`email`) and the last four digits of the credit card number (`lastCCFourDigits`) used to make the booking to get all associated bookings, or
- an email address (`email`) and `itemId`

...in that order

For `"Failed"` items, display a message that communicates the following information to your customers:

<pre>"The booking has failed either because this tour or activity was not available or there was a technical issue. Please contact Customer Service if you need more information."</pre>

**See also**: [Booking and item status list](../../appendices/#bookingStatus-field-values-and-meanings)

### Departure details

Departure details, such as the `departurePoint`, `departurePointAddress` and `departurePointDirections` will be included in the response. These fields may contain HTML escape characters, such as `&amp;` and special characters that are escaped using a backslash. Ensure that these fields are parsed after receiving the response or it may cause your JSON to be invalid.

**Example response object** ([/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings)):

```javascript
{
  "errorReference": null,
  "data": {
    "sortOrder": 0,
    "rulesApplied": null,
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
      "rulesApplied": null,
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
      },
      "travellerAgeBands": [{
        "sortOrder": 0,
        "count": 2,
        "pluralDescription": "Adults",
        "description": "Adult",
        "ageBandId": 1
      }],
      "voucherKey": "1000308214:899757cf8b419ed11f39045636b0b30af986d19126d04547097f4b9c05fb4b69:700179574",
      "voucherURL": "https://viatorapi.sandbox.viator.com/service/merchant/voucher.jspa?code=1000308214:899757cf8b419ed11f39045636b0b30af986d19126d04547097f4b9c05fb4b69:700179574&embedResources=false",
      "voucherRequirements": "You must present a paper voucher for this tour. We will email a link to access and print your voucher at the Lead Travelers email address.",
      "productPulledDown": false,
      "merchantCancellable": true,
      "productWidgetList": null,
      "savingAmount": 0,
      "vouchers": null,
      "passbooks": null,
      "termsAndConditions": null,
      "itineraryId": 1000308214,
      "productCode": "2065CPT",
      "tourGradeCode": "1DAY",
      "distributorItemRef": "distroItemRefJDP1006151732",
      "languageServicesLanguageCode": "en",
      "travelDate": "2015-09-03",
      "price": 26.28,
      "leadTravellerSurname": "Test",
      "barcodeOption": "tour",
      "barcodeType": "code128",
      "destId": 318,
      "voucherOption": "VOUCHER_PAPER_ONLY",
      "productTitle": "City Sightseeing Cape Town Hop-On Hop-Off Tour",
      "itemId": 700179574,
      "obfsId": 27643,
      "departurePoint": "Tour starts at V&amp;A Waterfront, outside the Two Oceans Aquarium, however you may board the bus at any one of the stops throughout the city (see the Itinerary section below for a list of stops)",
      "departurePointAddress": "",
      "departurePointDirections": "",
      "leadTravellerTitle": "Mr",
      "leadTravellerFirstname": "Homer",
      "lastRetailPrice": 26.28,
      "bookingEngineId": "UF",
      "priceFormatted": "$26.28",
      "savingAmountFormatted": "",
      "merchantNetPrice": 26.28,
      "merchantNetPriceFormatted": "$26.28",
      "currencyCode": "USD",
      "lastRetailPriceFormatted": "$26.28",
      "departsFrom": "Cape Town, South Africa",
      "tourGradeDescription": "1-Day Bus Tour (1DAY)",
      "hoursConfirmed": 0,
      "priceUSD": 26.28
    }],
    "voucherURL": "https://viatorapi.sandbox.viator.com/service/merchant/voucher.jspa?code=1000308214:899757cf8b419ed11f39045636b0b30af986d19126d04547097f4b9c05fb4b69&embedResources=false",
    "voucherKey": "1000308214:899757cf8b419ed11f39045636b0b30af986d19126d04547097f4b9c05fb4b69",
    "bookerEmail": "jocelyn@viator.com",
    "itineraryId": 1000308214,
    "exchangeRate": 1,
    "distributorRef": "distroRefJDP1006151732",
    "totalPrice": 26.28,
    "bookingDate": "2015-06-10",
    "totalPriceFormatted": "$26.28",
    "totalPriceUSD": 26.28,
    "hasVoucher": true,
    "userId": null,
    "currencyCode": "USD"
  },
  "dateStamp": "2015-06-10T00:33:24+0000", "errorType": null,
  "errorMessage": null,
  "errorName": null,
  "success": true,
  "totalCount": 1,
  "vmid": "321004",
  "errorMessageText": null
}
```