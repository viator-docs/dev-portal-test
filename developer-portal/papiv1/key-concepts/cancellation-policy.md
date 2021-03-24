# Cancellation policy

As well as *making* bookings, merchant partners are also able to *cancel* bookings through the Viator API using the [/bookings/cancel-reasons](../../../../openapi/reference/operation/cancellationReasons), [/bookings/{booking-ref}/cancel-quote](../../../../openapi/reference/operation/cancelBookingQuote) and [/bookings/{booking-ref}/cancel](../../../../openapi/reference/operation/cancelBooking) endpoints. Items cancelled via the [/bookings/{booking-ref}/cancel](../../../../openapi/reference/operation/cancelBooking) endpoint will be cancelled in full, and only one booking can be cancelled at a time.

For more information about the content of the new `merchantTermsAndConditions` object, see [Cancellation policy](../key-concepts/cancellation-policy).


## Cancellation policies

All products can be cancelled by the merchant; however, the refund granted by the supplier to the customer differs depending on the cancellation policy for the product in question.

There are <u>three</u> cancellation policy categories, **standard**, **custom** and **all sales final**, represented by an integer in the `merchantTermsAndConditionsType` field in the `merchantTermsAndConditions` object returned by [/product](../../../../openapi/reference/operation/product): `1`, `2` or `3`, respectively.

**Note:** *These policies are those provided by Viator to our merchant partners. Merchants can choose whether to extend these terms to their customers unchanged or set their own cancellation terms. For example, the merchant partner can choose to make all products non-refundable; or, they might change the full-refund cancellation window to 72 hours instead of 24 hours, and so forth.*

## `1` – Standard cancellation policy
Products in this category are cancellable up to 24 hours before the travel date (local supplier time) for a full refund. However, a <u>100% cancellation penalty</u> applies for cancellations submitted less than 24 hours before the start time. Most products (about 85%) fall into this category.

### Example response snippet

- **Source endpoint**: [/product](../../../../openapi/reference/operation/product)
- **Product**: `5010SYDNEY`

```javascript
{
  "data": {
    "merchantTermsAndConditions": {
      "termsAndConditions": "For a full refund, cancel at least 24 hours in advance of the start date of the experience.",
      "merchantTermsAndConditionsType": 1,
      "amountRefundable": null,
      "cancellationFromTourDate": [
        {
          "dayRangeMin": 0,
          "dayRangeMax": 1,
          "percentageRefundable": 0,
          "policyStartTimestamp": null,
          "policyEndTimestamp": null
        },
        {
          "dayRangeMin": 1,
          "dayRangeMax": null,
          "percentageRefundable": 100,
          "policyStartTimestamp": null,
          "policyEndTimestamp": null
        }
      ]
    },
    "...": "..."
  }
}
```

This product has the *standard* cancellation policy; i.e., when a booking is cancelled:

| Policy | **dayRangeMin** | **dayRangeMax** | Logic | **percentageRefundable** |
|--------|:-----------:|:-----------:|-------|:--------------------------:|
| *less than* <u>one</u> day (24 hours) before the start time | 0 | 1 | (product_start_time - cancellation_time) >= 0 days && (product_start_time - cancellation_time) < 1 days | 0 |
| *more than* <u>one</u> day (24 hours) before the start time | 1 | null | (product_start_time - cancellation_time) >= 1 day | 100 |

## `2` – Custom cancellation policy
The refund amount for products in this category varies depending on how long before its start time the product is cancelled. Many products on a custom policy are multi-day tours, which require more sophisticated planning on the supplier’s end. Only a small number of products (around 5%) fall into this category.

### Example response snippet

- **Source endpoint**: [/product](../../../../openapi/reference/operation/product)
- **Product**: `2264RJ410`

```javascript
"data": {
  "merchantTermsAndConditions": {
    "termsAndConditions": "If you cancel at least 30 day(s) in advance of the scheduled departure, there is no cancellation fee.<br>If you cancel between 10 and 29 day(s) in advance of the scheduled departure, there is a 50 percent cancellation fee.<br>If you cancel within 9 day(s) of the scheduled departure, there is a 100 percent cancellation fee.<br>",
    "merchantTermsAndConditionsType": 2,
    "amountRefundable": null,
    "cancellationFromTourDate": [
      {
        "dayRangeMin": 10,
        "dayRangeMax": 30,
        "percentageRefundable": 50,
        "policyStartTimestamp": null,
        "policyEndTimestamp": null
      },
      {
        "dayRangeMin": 30,
        "dayRangeMax": null,
        "percentageRefundable": 100,
        "policyStartTimestamp": null,
        "policyEndTimestamp": null
      },
      {
        "dayRangeMin": 0,
        "dayRangeMax": 10,
        "percentageRefundable": 0,
        "policyStartTimestamp": null,
        "policyEndTimestamp": null
      }
    ]
  },
  "...": "..."
}
```

This product has a complex cancellation policy; where cancellations processed:

| Policy | **dayRangeMin** | **dayRangeMax** | Logic | **percentageRefundable** |
|--------|:-----------:|:-----------:|-------|:--------------------------:|
| <u>30</u> days or more before the start time | 30 | null | (product_start_time - cancellation_time) >= 30 days | 100 |
| <u>10</u> days and *less than* <u>30</u> days (10 to 30 days) *before* the start time or more | 10 | 30 | (product_start_time - cancellation_time) >= 10 days && (product_start_time - cancellation_time) < 30 days | 50 |
| *less than* <u>10</u> days *before* the start time | 0 | 10 | (product_start_time - cancellation_time) < 10 days | 0 |

**Note:** `null` in the `dateRangeMax` field means *negative infinity*; i.e., *infinitely far in the past with respect to* `dateRangeMin`.

Additional clauses will be included in the `termsAndConditions` field in natural language. This field is for human consumption and is not classically machine-interpretable.

## `3` – All sales final (100% cancellation penalty / no refund offered)

Products in this category cannot be cancelled or amended without incurring a 100% penalty; i.e., the refund amount will be zero. Around 10% of products fall into this category.

### Example response snippet

- **Source endpoint**: [/product](../../../../openapi/reference/operation/product)
- **Product**: `5985P7`

```javascript
{
  "data": {
    "merchantTermsAndConditions": {
      "termsAndConditions": "All sales are final and incur 100% cancellation penalties.<br>",
      "merchantTermsAndConditionsType": 3,
      "amountRefundable": null,
      "cancellationFromTourDate": [
        {
          "dayRangeMin": 0,
          "dayRangeMax": null,
          "percentageRefundable": 0,
          "policyStartTimestamp": null,
          "policyEndTimestamp": null
        }
      ]
    },
    "...": "..."
  }
}
```
Products in this category can be cancelled, but no refund will be granted (in most cases...)

## Canceling items with a 'pending' booking status

As alluded, there is an exception to this rule. Products with an 'on-request' [booking type](../key-concepts/booking) can still be cancelled when their booking status is `"pending"` – i.e., before the supplier has confirmed the booking – and a full refund will be granted.

It is impossible to predict how long an 'on-request' booking will remain 'pending'. However, it is possible to check by enquiring about the booking using one of the post-booking services; i.e.:

* [/booking/status](../../../../openapi/reference/operation/bookingStatus)
* [/booking/status/items](../../../../openapi/reference/operation/bookingStatusItems)
* [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking)
* [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings)

An 'all sales final' product in a 'pending' state that can be cancelled and a refund granted will have the following characteristics:

1. The `bookingStatus` object returned from one of the services above will have a `type` of `"PENDING"`, and `pending` will be `true`.
2. The `amountRefundable` field of the `merchantTermsAndConditions` object will be non-zero and non-null. Rather, it will contain a currency-formatted string showing the amount that would be refunded if the cancellation were performed immediately; e.g., "USD 55.33".


## Policy start and end times

Within the `merchantTermsAndConditions` object returned in the response from [/booking/book](../../../../openapi/reference/operation/bookingBook), [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking) and [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings), the `amountRefundable` field shows the amount of money in the selected currency that will be refunded if the cancellation is processed now, while the `policyStartTimestamp` and `policyEndTimestamp` fields indicate the exact times between which the different cancellation refund rates apply.

### Example response snippet (`merchantTermsAndConditions`) from [/booking/book](../../../../openapi/reference/operation/bookingBook)

- **Product**: `5010SYDNEY`
- **Note**: observe that `amountRefundable`, `policyStartTimestamp` and `policyEndTimestamp` are populated here.

```javascript
"data": {
  "merchantTermsAndConditions": {
    "termsAndConditions": "For a full refund, cancel at least 24 hours in advance of the start date of the experience.",
    "amountRefundable": "USD 55.33",
    "cancellationFromTourDate": [
      {
        "dayRangeMin": 1,
        "dayRangeMax": null,
        "percentageRefundable": 100,
        "policyStartTimestamp": null,
        "policyEndTimestamp": 1551513600000
      },
      {
        "dayRangeMin": 0,
        "dayRangeMax": 1,
        "percentageRefundable": 0,
        "policyStartTimestamp": 1551340800000,
        "policyEndTimestamp": 1551427200000
      }
    ]
  },
  "...": "..."
}
```

## Post-travel cancellations

Occasionally, customers seek a refund for a product **after** completing their travels.

The reason for this might be because they were unable to attend the tour due to the supplier having cancelled the tour due to bad weather or some other reason out of the customer's control; or, the customer might have been extremely dissatisfied with the tour itself, felt that it was misrepresented in its advertising, or some other serious complaint.

When this occurs, you will need to [send a refund request by email to dpsupport](mailto:dpsupport@viator.com) and include both "CANCEL" and the booking reference number in the subject line.

For **all** post-travel cancellation requests, you will need to include a detailed description of the issue. 

Except in cases of known service interruptions (e.g., due to extreme weather events), we will first verify the issue and seek authorization from the product supplier. 

Once a decision regarding the refund has been made, we will notify your Customer Services Department with this information. You will then need to advise your customer directly and process the refund if granted.

## Interpreting `policyStartTimestamp` and `policyEndTimestamp`

The integers that populate the `policyStartTimestamp` and `policyEndTimestamp` fields represent points in time that mark the boundaries of the policy time period in the [Unix time format](https://en.wikipedia.org/wiki/Unix_time); i.e., the number of seconds that have elapsed since 00:00:00 Thursday, 1 January 1970, Coordinated Universal Time (UTC), minus leap seconds.

Unix timestamps can be easily read and interpreted using the 'time' (or similar) library of your favorite programming language. For human purposes, an [online conversion tool](https://www.epochconverter.com/) can be used.

As per the example above, canceling this booking between the following times yields zero refund (because it is within the 24 hour window):

| **Field name** | `policyStartTimestamp` | `policyEndTimestamp` |
|----------------|------------------------|----------------------|
| **Unix time** | 1551340800000 | 1551427200000 |
| **Human readable time** | GMT: Thursday, February 28, 2019 8:00:00 AM | GMT: Friday, March 1, 2019 8:00:00 AM |

* **Note**: Please use `policyStartTimestamp` and `policyEndTimestamp`, rather than `dayRangeMin` and `dayRangeMax`, to determine which cancellation policy is in effect.

## Partial refunds

While we recommend that you, as a merchant partner, support the processing of partial refunds for your customers, it is up to your whether you implement this functionality. 

If you would prefer to only grant the full (100%) refund that is offered on most products so long as the cancellation is processed more than 24 hours prior to the product's start time, we recommend that you implement logic that checks whether a 100% refund is available for the product at the time the customer wishes to cancel their booking.

| **Type 1: Standard policy** (`merchantTermsAndConditionsType` is `1`) |
|-------------------------------------------------------------|
| The 100% refund is available so long as the cancellation is performed more than 24 hours prior to the product start time |

| **Type 2: Custom policy** (`merchantTermsAndConditionsType` is `2`) |
|-----------------------------------------------------------|
| You will need to check whether any of the object-items in the `cancellationFromTourDate` array have: <ul><li>a `percentageRefundable` value of `100`, and</li><li>`dayRangeMin` and `dayRangeMax` **or** `policyStartTimestamp` and `policyEndTimestamp` values that include the present time</li></ul> |

| **Type 3: All sales final** (`merchantTermsAndConditionsType` is `3`) |
|-------------------------------------------------------------|
| No refunds are available; therefore, granting a refund to your customer for this kind of product will be solely at *your* expense (i.e., you will still be invoiced for the cost of the tour by Viator). Therefore, we recommend that you do not allow refunds for products with this policy. |