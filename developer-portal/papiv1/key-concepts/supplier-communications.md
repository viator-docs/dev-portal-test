# Supplier communications

## How can suppliers communicate with end customers?

Suppliers occasionally need to reach out to customers for a variety of reasons, such as:

* Requesting pick-up locations, flight details or passenger weight information
* Providing weather alerts, sold-out notifications or general messaging

To allow suppliers to contact customers directly, Viator provides a **Closed Loop Communication (CLC)** system. 

## How to enable CLC

CLC is enabled per-booking and at the time of booking by supplying the customer’s `email` and either `homePhone` or [`cellphone` + `cellPhoneCountryCode`] – or both – in the `booker` object in the request body sent to the [/booking/book](../../../../openapi/reference/operation/bookingBook) service when making a booking.

This will allow suppliers to send CLC messages directly to the end customer.

### Note:

* You will receive a CC of each supplier message to your customer support email address in case further assistance is required, but no action from your support team will be necessary for suppliers to communicate with customers.

* Merchants choosing this option should mention to their customers that they are purchasing a product from a third-party supplier, and that they may therefore receive communications regarding the purchase directly from that supplier. 

### Example request body snippet to enable direct CLC
```javascript
{
    ...
    "booker": {
        "homePhone": "(02)66987564",
        "firstname": "Homer Test",
        "surname": "Simpson Test",
        "title": "Mr",
        "cellPhoneCountryCode": "61",
        "cellPhone": "431532778",
        "email": "hsimpson@customeremail.com"
    }
    ...
}
```

## Supplier communications without CLC

To have CLCs from the supplier sent <u>only</u> to your (the merchant's) customer support team:

* Leave the `cellPhone`, `cellPhoneCountryCode` and `homePhone` fields blank in the request to the [/booking/book](../../../../openapi/reference/operation/bookingBook) service when making a booking.

**Note:** Utilizing this option requires merchants to manage the final loop of communication with the end customer to ensure that their tour/activity can be fulfilled successfully.