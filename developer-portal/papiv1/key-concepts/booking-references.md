# Booking references 

When a booking is made successfully via the [/booking/book](../../../../openapi/reference/operation/bookingBook) endpoint, Viator assigns it a numeric identifier, now known as the **booking reference**.

This booking reference is returned in the service's response in the `itemId` field; however, this `itemId` is found in different locations depending on the endpoint used:

| Endpoint |  itemId element location |
|----------|-------------------|
| [/booking/book](../../../../openapi/reference/operation/bookingBook) | `data.itemSummaries[].itemId` |
| [/booking/status](../../../../openapi/reference/operation/bookingStatus) | `data.itemSummaries[].itemId` |
| [/booking/status/items](../../../../openapi/reference/operation/bookingStatusItems) | `data[].itemId` |
| [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking) | `data.itemSummaries[].itemId` |
| [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) | `data[].itemSummaries[].itemId` |

The booking reference can used in the **request** in the following endpoints as the value in the `itemId` field or in the `itemIds` array:

- [/booking/status](../../../../openapi/reference/operation/bookingStatus)
- [/booking/status/items](../../../../openapi/reference/operation/bookingStatusItems)
- [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking)
- [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings)
- [/booking/voucher](../../../../openapi/reference/operation/bookingVoucher)

## New booking references

The new booking cancellation endpoints; i.e.: 

- [/bookings/{booking-reference}/cancel-quote](../../../../openapi/reference/operation/cancelBookingQuote)
- [/bookings/{booking-reference}/cancel](../../../../openapi/reference/operation/cancelBooking)

...use this booking reference value as an in-URL request parameter, but its format is slightly different. 

Essentially, it is the booking's numeric identifier (`itemId`), but prepended with `BR-`.  For example, if the `itemId` is `580254558`, the `bookingId` value in the cancellation request should be `BR-580254558`. 

The booking cancellation endpoints confirm the booking reference in the response in the `bookingId` field; e.g.:

```json
{
  "bookingId": "BR-580669678",
  "refundDetails": {
    "itemPrice": 412.04,
    "refundAmount": 412.04,
    "refundPercentage": 100,
    "currencyCode": "USD"
  },
  "status": "CANCELLABLE"
}
```