# Get the tour grade pricing matrix

The [/booking/pricingmatrix](../../../../openapi/reference/operation/bookingPricingmatrix) service retrieves the pricing matrix for tour grades, product age bands and pax (passenger) mixes.

**Example request object** ([/booking/pricingmatrix](../../../../openapi/reference/operation/bookingPricingmatrix))

```javascript
  "productCode": "5261HTLAP",
  "tourGradeCode": "Zone 1",
  "bookingDate": "2013-12-01",
  "currencyCode": "USD",
  "specialReservation": false
```

`bookingDate`: The date to check for pricing data. This is an optional parameter for a normal product.

If the date is not provided then the nearest available date is determined (i.e. not blocked out or unavailable for any reason)

**Example response object** ([/booking/pricingmatrix](../../../../openapi/reference/operation/bookingPricingmatrix))

```javascript
{
  "data": [{
    "pricingUnit": "per person",
    "bookingDate": "2013-12-01",
    "sortOrder": 1,
    "ageBandPrices": [{
      "bandId": 1,
      "prices": [{
        "sortOrder": 1,
        "currencyCode": "USD",
        "price": 81.94,
        "priceFormatted": "$81.94",
        "merchantNetPrice": 65.44,
        "merchantNetPriceFormatted": "$65.44",
        "minNoOfTravellersRequiredForPrice": 1
      }],
      "sortOrder": 1,
      "minimumCountRequired": 1,
      "maximumCountRequired": 1
    }]
  },
  {
    "pricingUnit": "per person",
    "bookingDate": "2013-12-01",
    "sortOrder": 2,
    "ageBandPrices": [{
      "bandId": 1,
      "prices": [{
        "sortOrder": 1,
        "currencyCode": "USD",
        "price": 40.97,
        "priceFormatted": "$40.97",
        "merchantNetPrice": 32.73,
        "merchantNetPriceFormatted": "$32.73",
        "minNoOfTravellersRequiredForPrice": 1
      }],
      "sortOrder": 1,
      "minimumCountRequired": 2,
      "maximumCountRequired": 2
    }]
  },
  {
    "pricingUnit": "per person",
    "bookingDate": "2013-12-01",
    "sortOrder": 3,
    "ageBandPrices": [{
      "bandId": 1,
      "prices": [{
        "sortOrder": 1,
        "currencyCode": "USD",
        "price": 27.32,
        "priceFormatted": "$27.32",
        "merchantNetPrice": 21.81,
        "merchantNetPriceFormatted": "$65.44",
        "minNoOfTravellersRequiredForPrice": 1
      }],
      "sortOrder": 1,
      "minimumCountRequired": 3,
      "maximumCountRequired": 3
    }]
  }],
  "errorReference": null,
  "dateStamp": "2017-11-24T21:30:47+0000",
  "errorType": null,
  "errorCodes": null,
  "errorMessage": null,
  "errorName": null,
  "success": true,
  "totalCount": 3,
  "errorMessageText": null,
  "vmid": "321050"
}
```

**Description of elements in JSON response object**

| Object | Element | Type | Comments | To be viewed by customer | Required field |
|--------|---------|------|----------|:------------------------:|:--------------:|
| `data`   | | object | main response object | ❌ | ✅ |
|  | `sortOrder` | integer | order in which to show the pricing | ✅ | ✅ |
|  | `bookingDate` | date | booking date criteria | ✅ | ✅ |
|  | `pricingUnit` | string | unit for pricing: currently, only "per person" is supported | ✅ | ✅ |
| `ageBandPrices` | | object | available age bands and their pricing | ❌ | ✅ |
| | `sortOrder` | integer | sort order for age band display | ✅ | ✅ |
| | `bandId` | integer | **Note**: the numeric `bandId` is associated with an age band description (e.g., `"Adult"`, `"Infant"` etc.) and a corresponding age range (e.g., from 12 to 99) - these details are available from the [/product](../../../../openapi/reference/operation/product) service. See [Working with age bands](../../key-concepts/working-with-age-bands) | ❌ | ✅ |
| | `minimumCountRequired` | integer | minimum number of pricing units that apply to these prices | | ✅ |
| | `maximumCountRequired` | integer | maximum number of pricing units that apply to these prices | | ✅ |
| `prices` | | object | pricing available for the age band (based on the min and max count requirements) | ✅ | ✅ |
| | `currencyCode` | string | currency of the pricing | ✅ | ✅ |
| | `sortOrder` | integer | order the pricing is to be shown within the `bandId` | ✅ | ✅ |
| | `price` | number | price in decimal format (for merchant API partners, this is the 'suggested sell price') | ❌ | ✅ |
| | `priceFormatted` | string | suggested sell price formatted according to the currency selected (with two decimal places where applicable) | ✅ | ✅ |
| | `merchantNetPrice` | number | merchant net price in decimal format | ❌ | ✅ |
| | `merchantNetPriceFormatted` | string | merchant net price formatted according to the selected currency | ❌ | ✅ |
| | `minNoOfTravellersRequiredForPrice` | integer | number of units that the pricing applies to (e.g., a `minNoOfTravellersRequiredForPrice` of `3` means that the price is for three people) | ✅ | ✅ |
