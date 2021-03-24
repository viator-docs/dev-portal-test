# Reviewing bookings

The [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking) service gets the details of a single specific past booking based on the `voucherKey` or `itemId` passed into it.

## Sample query Parameters

`"email=john.doe@viator.com&itemId=600033670"`

or

`"voucherKey=3299307:93c7f36a56b18ba1068787ba7fb7988da5c8ad08db77604110141ff21498603e:600033670"`

## Key concepts
### Email

The email address passed must match the email address associated to the itinerary

### Departure Details

Departure details, such as the `departurePoint`, `departurePointAddress` and `departurePointDirections`, will be included in the response. These fields may contain HTML escape characters, such as `&amp;` and special characters that are escaped by a backslash. Ensure that these fields are parsed after receiving the response, or it may cause your JSON to be invalid.


**Example response object** ([/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking)):

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
        "text": "Paid &amp; Confirmed", "type": "CONFIRMED",
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
      "savingAmountFormated": "",
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
  }
}

```
