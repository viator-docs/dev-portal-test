# Working with age bands

## Age bands

The available age bands for a product, such as *adult*, *child*, *infant*, etc., are returned by the [/product](../../../../openapi/reference/operation/product) service. The customer can select a different number of people from each age band during the price check and checkout process.

## Why have age bands?

Tour and experience product operators can set different prices for (and impose different rules on) those wishing to make a booking for their product according to how old they are. 

For example, suppliers might choose to charge people 18 years and older ('adults') the full ticket price, while 'children' can book at a lower price. 

Or, the tour operator may only allow children to make a group booking for the tour so long as the group contains 'at least one adult'.

Viator provides five categories (age bands) that product operators can use to segregate travelers into age groups (the limits of which they also define) in order to set pricing and traveler-count participation rules for their product according to the age band categories.

## Supported age band categories

The age bands supported by the Viator API are as follows:

| `bandId` | Description |
|:------:|-------|
| **1** | Adult |
| **2** | Child |
| **3** | Infant |
| **4** | Youth |
| **5** | Senior |

The names and corresponding numeric identifiers of these categories are fixed as in the table above (i.e., `1` is always `Adult`); however, the exact age range to which each category pertains must be defined manually by the supplier.

The maximum and minimum ages that each age band describes for each product can be retrieved from the [/product](../../../../openapi/reference/operation/product) service.

## Example of age band definitions

For example, a call to [/product](../../../../openapi/reference/operation/product) regarding `10040WORLD` yields the following `ageBands` array within its response object:

```javascript
"ageBands": [
  {
    "sortOrder": 1,
    "ageFrom": 13,
    "ageTo": 64,
    "adult": true,
    "bandId": 1,
    "pluralDescription": "Adults",
    "treatAsAdult": true,
    "count": 0,
    "description": "Adult"
  },
  {
    "sortOrder": 2,
    "ageFrom": 65,
    "ageTo": 99,
    "adult": false,
    "bandId": 5,
    "pluralDescription": "Seniors",
    "treatAsAdult": true,
    "count": 0,
    "description": "Senior"
  },
  {
    "sortOrder": 3,
    "ageFrom": 4,
    "ageTo": 12,
    "adult": false,
    "bandId": 2,
    "pluralDescription": "Children",
    "treatAsAdult": false,
    "count": 0,
    "description": "Child"
  },
  {
    "sortOrder": 4,
    "ageFrom": 0,
    "ageTo": 3,
    "adult": false,
    "bandId": 3,
    "pluralDescription": "Infants",
    "treatAsAdult": false,
    "count": 0,
    "description": "Infant"
  }
```

For this product, the age bands have been defined as follows:

| `bandId` | `description` | `ageFrom` | `ageTo` |
|:--------:|:-----------:|:-------:|:-----:|
| **1** | Adult | 13 | 64 |
| **5** | Senior | 65 | 99 |
| **2** | Child | 4 | 12 |
| **3** | Infant | 0 | 3 |

Product operators must define at least one age band for their tour, and there are no 'default' age ranges. Therefore, if the product operator has only specified a single 'adult' age band covering ages 18-99, it must be assumed that only people aged 18-99 are eligible to book the tour, essentially excluding children and centenarians in this case.

| Field | Type | Definition |
|-------|------|------|
| `sortOrder` | integer | the sort order for *this* age band |
| `adult` | boolean | `true` if this age band is the 'adult' age band |
| `treatAsAdult` | boolean | `true` if this age band can book the tour without the need for adult accompaniment |

**Note**: `bandId` must be supplied in the request body of the following services:

- [/booking/availability](../../../../openapi/reference/operation/bookingAvailability)
- [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades)
- [/booking/calculateprice](../../../../openapi/reference/operation/bookingCalculateprice)
- [/booking/book](../../../../openapi/reference/operation/bookingBook)

Age bands are referenced by their `bandId` in the responses from the following services:

- [/product](../../../../openapi/reference/operation/product)
- [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades)
- [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix)
- [/booking/book](../../../../openapi/reference/operation/bookingBook)
- [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking)
- [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings)
- [/booking/calculateprice](../../../../openapi/reference/operation/bookingCalculateprice)

