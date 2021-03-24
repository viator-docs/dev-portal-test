# Cancellation API workflow

## Note:

- All booking cancellations (except for those requested after the date of travel) must now be performed via the API. Viator no longer offers ordinary cancellation services via our customer support function.
- To cancel a booking after the tour or activity has occurred, please contact [Viator Partner Support](mailto:dpsupport@viator.com)
- **Legacy API keys:** Bookings made via the [v1 /booking/book](../../../openapi/reference/operation/bookingBook) endpoint using a [v1 legacy API key](../../authentication/#legacy-api-keys) can be canceled using the [v2 cancellation endpoint](../../../../openapi/reference/operation/cancelBooking) and a [v2 API key](../../authentication/#api-key) so long as the [legacy (v1) API key](../../authentication/#legacy-api-key) used to make the booking is linked to the partner's [v2 API key](../../authentication/#api-key).


## Getting cancellation reasons
<a id="getting-cancellation-reasons"></a>

When canceling a booking, you are required to submit a valid 'reason for the cancellation' to assist with Viator's internal processes. This is accomplished via the inclusion of a valid reason code in the body of the request. The reason codes can be retrieved from the [/bookings/cancel-reasons](../../../../openapi/reference/operation/cancellationReasons) endpoint.

As the acceptable reasons for cancellation may be extended at any point (existing reasons will not change or be removed), we recommend retrieving an up-to-date list from this endpoint at least weekly.

The output from the [/bookings/cancel-reasons](../../../../openapi/reference/operation/cancellationReasons) endpoint at the time of writing is as follows:

```javascript
{
    "reasons": [
        {
            "cancellationReasonCode": "Customer_Service.I_canceled_my_entire_trip",
            "cancellationReasonText": "I canceled my entire trip"
        },
        {
            "cancellationReasonCode": "Customer_Service.Booked_wrong_tour_date",
            "cancellationReasonText": "Booked wrong tour/date"
        },
        {
            "cancellationReasonCode": "Customer_Service.Duplicate_Booking",
            "cancellationReasonText": "Duplicate Booking"
        },
        {
            "cancellationReasonCode": "Customer_Service.Chose_a_different_cheaper_tour",
            "cancellationReasonText": "Chose a different/cheaper tour"
        },
        {
            "cancellationReasonCode": "Customer_Service.Weather",
            "cancellationReasonText": "Weather"
        },
        {
            "cancellationReasonCode": "Customer_Service.Unexpected_medical_circumstances",
            "cancellationReasonText": "Unexpected/medical circumstances"
        },
        {
            "cancellationReasonCode": "Customer_Service.Tour operator asked me to cancel",
            "cancellationReasonText": "Tour operator asked me to cancel"
        }
    ]
}
```

## Canceling a booking

### Getting a cancellation quote
<a id="get-a-booking-cancellation-quote"></a>

Before canceling the booking, call the [/bookings/{booking-reference}/cancel-quote](../../../../openapi/reference/operation/cancelBookingQuote) endpoint to get information about whether the booking can be canceled using this endpoint and what the refund will be, for example:

```html
GET https://api.viator.com/partner/bookings/BR-580254558/cancel-quote
```

**Note**: For information about the **{booking-reference}** in URL parameter, see [Key concepts: Booking references](../../key-concepts/booking-references)

You will receive a cancellation quote object, e.g.:

```javascript
{
    "bookingId": "BR-580254558",
    "status": "CANCELLABLE",
    "refundDetails": {
        "itemPrice": 109.77,
        "refundAmount": 109.77,
        "currencyCode": "USD",
        "refundPercentage": 100.00
    }
}
```

 **Note**: Bookings that have not been confirmed by the supplier and have a status of `"PENDING"` will report an `itemPrice`, `refundAmount` and `refundPercentage` of `0`, as no fees are charged until the booking's status is `"CONFIRMED"`.

The data elements in this object have meanings as follows:

| Element | Meaning | Example |
|---------|---------|---------|
| `bookingId` | the booking reference number prepended with `BR-` | `BR-580254556` |
| `status` | One of the following: <ul><li>`CANCELLABLE`: the booking is eligible to be cancelled</li><li>`CANCELLED`: the booking has already been cancelled</li><li>`NOT_CANCELLABLE`: the booking is for a product that operated in the past, and therefore cannot be cancelled using this endpoint (you will need to [send an email to dpsupport](mailto:dpsupport@viator.com) including both "CANCEL" and the booking reference number in the subject line in order to request a refund for such a booking)</li></ul> | `CANCELLABLE` |
| `refundDetails` | object containing information about the refund | |
| `itemPrice` | the **merchant net price** + **transaction fee** for this product at the time of booking in the currency specified by `currencyCode` | `109.77` |
| `refundAmount` | the amount that will be deducted from your invoice if the booking is cancelled now | `109.77` |
| `currencyCode` | the currency code for the currency in which pricing information is displayed | `USD` |
| `refundPercentage` | the refund amount expressed as a percentage of the `itemPrice` | `100.00` |

### Canceling the booking
<a id="cancel-the-booking"></a>

If the `status` field has a value of `CANCELLABLE` and you are happy with the `refundAmount`, call the [/bookings/{booking-ref}/cancel](../../../../openapi/reference/operation/cancelBooking) endpoint to cancel the booking, e.g.:

```html
POST https://api.viator.com/partner/bookings/BR-580254558/cancel
```

A reason code corresponding to the reason for cancellation must be included in the request body; e.g.:

```javascript
{
  "reasonCode":"Customer_Service.Chose_a_different_cheaper_tour"
}
```

You should receive a response indicating that the cancellation was successful, e.g.:

```javascript
{
    "bookingId": "BR-580254558",
    "status": "ACCEPTED"
}
```

A `status` of `ACCEPTED` indicates that the booking was successfully canceled.