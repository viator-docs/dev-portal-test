<style type='text/css'>
figure {
  width: 100%;
  text-align: center;
  font-style: italic;
  font-size: smaller;
  text-indent: 0;
  border: thin silver solid;
  margin: 0.5em;
  padding: 0.5em;
}
figcaption {  
    padding-top: 10px;
}
</style>

# Back-end checks

These checks will involve:

- Summarizing your **content ingestion** and **booking** processes, including 
- Making various types of bookings and having these bookings assessed to ensure correctness and completeness

## Content ingestion process

We would like you to summarize how you have implemented your content ingestion and caching process.

Are you are periodically ingesting content – e.g., product lists and pricing details – and loading this into a local database, or are you calling Viator's servers and caching the replies?

- Which endpoints do you call when ingesting content?
- How often are you making calls to these endpoints?
- What is the total number of products you have been able to download using these methods?

## Booking process

We would also like a summary of your booking process. Please write a short summary of the endpoints you use in your booking workflow, from product search to completing the booking.

## Bookings checks

As well as meeting the various requirements regarding your booking platform's front end mentioned in the front-end site certification section, we will also need to verify that your system is correctly fulfilling its requirements with regard to bookings. 

All checks must be passed before we can issue you a production-level apiKey that will allow you to start making real bookings.

All tests in this section evaluate booking records made with your sandbox apiKey to ensure that all essential data elements have been successfully registered in our database.

For information on how to make and check bookings, see the following sections in the technical manual for merchant partners:

- [Booking concepts](../../../papiv1/key-concepts/booking-concepts)
- [Booking process flow](../../../papiv1/workflows/booking-process-flow/)
- [Making a booking](../../../papiv1/workflows/making-a-booking/)
- [Reviewing bookings](../../../papiv1/workflows/reviewing-bookings/)
- [Checking bookings](../../../papiv1/workflows/checking-bookings/)

### Check 1: A booking has been made and confirmed

This test looks for a booking made with your apiKey that has a 'confirmed' status.

**How to verify**: When making a booking via the API using the [/booking/book](../../../../openapi/reference/operation/bookingBook) service, the booking's status will be communicated in the response object from this service; i.e.:

```javascript
"data": {
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
    ...
}
```

Alternatively, you can inspect the status of bookings using the following services:

- [/booking/status](../../../../openapi/reference/operation/bookingStatus)
- [/booking/status/items](../../../../openapi/reference/operation/bookingStatusItems)
- [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking)
- [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings)

**To pass the test**: As in the example above, the `type` field of the `bookingStatus` object must have the value `"CONFIRMED"`.

### Check 2: A booking has been successfully cancelled

This test looks for a booking made with your apiKey that has a 'cancelled' status. For information about cancelling bookings via the API, see [Cancellation API workflow](https://docs.viator.com/partner-api/merchant/technical/#section/Common-workflows-and-data-validation/Cancellation-API-workflow) in the technical manual for merchant partners.

**Instruction**: Once you have made a booking using the [/booking/book](../../../../openapi/reference/operation/bookingBook)) service, use [/bookings/{booking-reference}/cancel](../../../../openapi/reference/operation/cancelBooking) to cancel it.

**How to verify**: Check the item's status using any of the booking status services mentioned above. The `bookingStatus` object in the response should appear as follows:

```javascript
"data": {
    "bookingStatus": {
        "status": 3,
        "text": "Cancelled",
        "type": "CANCELLED",
        "level": "ITINERARY",
        "confirmed": false,
        "pending": false,
        "amended": false,
        "cancelled": true,
        "failed": false
    },
    ...
}
```
**To pass the test**: One booking made with your apiKey must have a status type of `"CANCELLED"`.

### Check 3: The customer's telephone number is being successfully recorded in each booking

This test checks whether the customer's telephone number was included in the `booker` object in the request body for the [/booking/book](../../../../openapi/reference/operation/bookingBook) service when making a booking.

Example of a correctly populated `booker` object:

```javascript
"booker": {
    "email": "bill.gates@microsoft.com",
    "homePhone": "+1 425-882-8080",
    "firstname": "William",
    "surname": "Gates III",
    "title": "Mr",
    "cellPhoneCountryCode": "+1",
    "cellPhone": "425-882-8080"
}
```

**How to verify**: Once a booking has been made, it is impossible to access the customer's telephone number via the API. However, if you have correctly populated the `homePhone` field of the `booker` object, your integration will certainly pass this test.

**To pass the test**: Ensure that the `homePhone` field in the booker object is populated when making a booking.

### Check 4: The customer's email address is being successfully recorded in each booking

This check focuses on whether the `booker` object passed to [/booking/book](../../../../openapi/reference/operation/bookingBook) has been correctly populated. In this case, we're testing to see if the customer's email was submitted via the `email` field.

**How to verify**: It **is** possible to verify this for yourself using one of the following services:

- [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking)
- [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings)

Specifically, check an existing booking and inspect the `bookerEmail` field in the responses from either of the abovementioned services. It should contain a valid email address; e.g.:

```javascript
"data": {
    "bookerEmail": "bill.gates@microsoft.com",
    ...
}
```

**To pass the test**: `bookerEmail` must contain a valid email address.

### Check 5: A booking with successfully completed booking questions has been made

All booking questions returned in the `bookingQuestions` array in the response from [/product](../../../../openapi/reference/operation/product) must be correctly supplied in the `bookingQuestionAnswers` array in the request to [/booking/book](../../../../openapi/reference/operation/bookingBook).

Example booking question in response from [/product](../../../../openapi/reference/operation/product):

```javascript
"data": {
    "bookingQuestions": [
        {
            "sortOrder": 1,
            "questionId": 23,
            "stringQuestionId": "weights_passengerWeights",
            "subTitle": "(e.g. 127 pounds, 145 kilos, etc)",
            "title": "Passenger Weights",
            "required": true,
            "message": "For safety reasons you must enter the weight of all passengers. Please indicate pounds or kilos."
        }
    ],
    ...
}
```

Example of a booking-question answer to a `bookingQuestion` in a request to [/booking/book](../../../../openapi/reference/operation/bookingBook):

```javascript
{
    "items": [
        {
            "bookingQuestionAnswers": [
                {
                    "questionId": 23,
                    "answer": "120 kgs"
                }
            ],
            ...
        }
    ],
    ...
}
```

**How to verify**: This criterion cannot be self-verified as there is no mechanism in the API to retrieve this data once it has been submitted.

**To pass the test**: Ensure that you submit answers (`bookingQuestionAnswers`)to all booking questions (`bookingQuestions`) when making the booking using [/booking/book](../../../../openapi/reference/operation/bookingBook).

### Check 6: A booking with successfully completed hotel pick-up information has been made

This test relates to products for which it is necessary to specify a hotel pick-up point. Such products return the following in the response from [/product](../../../../openapi/reference/operation/product):

```javascript
"data": {
    "hotelpickup": true,
    ...
}
```

When booking these products, you must ensure that the `hotelId` and `pickupPoint` fields in each booking item object are populated with valid data, as retrieved from the [/booking/hotels](../../../../openapi/reference/operation/bookingHotels) service.

For example, if the response from [/booking/hotels](../../../../openapi/reference/operation/bookingHotels) includes the following:

```javascript
"data": [
    ...
    {
        "sortOrder": 7,
        "productCodes": null,
        "destinationId": 684,
        "phone": "+1 123 456 7890",
        "city": "Las Vegas",
        "postcode": "89109-8933",
        "latitude": 36.115753,
        "longitude": -115.174446,
        "notes": null,
        "brand": null,
        "hotelString": null,
        "address": "3570 Las Vegas Blvd South, ",
        "name": "Caesars Palace",
        "id": "1091762"
    },
...
]
```

...then the `hotelId` and `pickupPoint` fields would be valid in the booking request sent to [/booking/book](../../../../openapi/reference/operation/bookingBook) if filled as follows:

```javascript
"items": [
    "hotelId": 1091762,
    "pickupPoint": "3570 Las Vegas Blvd South",
    ...
]
```

**How to verify**: This criterion cannot be self-verified as there is no mechanism in the API to retrieve this data once it has been submitted.

**To pass the test**: Ensure that you submit a pick up point when making the booking using [/booking/book](../../../../openapi/reference/operation/bookingBook).

### Check 7: A booking with 'special requirements' has been made

The `specialRequirements` field – part of the request object sent to [/booking/book](../../../../openapi/reference/operation/bookingBook) – is used to capture any additional requirements for the booking, such as dietary restrictions like "strict vegan" or mobility information, such as "wheelchair required".

You must provide a means for your customers to specify any such special requirements as part of the booking process. We will inspect your booking records to ensure that a `specialRequirements` field has been successfully submitted at least once.

Example [/booking/book](../../../../openapi/reference/operation/bookingBook) request object snippet:

```javascript
{
    "items": [
        {
            "specialRequirements": "allergic to nuts",
            ...
        }
    ],
    ...
}
```
**How to verify**: This criterion cannot be self-verified as there is no mechanism in the API to retrieve this data once it has been submitted.

**To pass the test**: Make at least one booking with a filled-in `specialRequirements` field using [/booking/book](../../../../openapi/reference/operation/bookingBook).

### Check 8: A booking with a standard cancellation policy has been successfully confirmed and cancelled

This test looks to see if a product with a 'standard' cancellation policy has been successfully confirmed and then cancelled by you.

The 'standard' cancellation policy is the most common type of cancellation policy for products in our inventory. The cancellation terms that apply to each product can be determined by inspecting the product information retrieved from the [/product](../../../../openapi/reference/operation/product) service.

Products with a standard cancellation policy will return a `merchantTermsAndConditions` object with contents as follows:

```javascript
"merchantTermsAndConditions": {
    "termsAndConditions": "For a full refund, cancel at least 24 hours in advance of the start date of the experience.",
    "merchantTermsAndConditionsType": 1,
    "amountRefundable": null,
    "cancellationFromTourDate": [
        {
            "dayRangeMin": 1,
            "dayRangeMax": null,
            "percentageRefundable": 100,
            "policyStartTimestamp": null,
            "policyEndTimestamp": null
        },
        {
            "dayRangeMin": 0,
            "dayRangeMax": 1,
            "percentageRefundable": 0,
            "policyStartTimestamp": null,
            "policyEndTimestamp": null
        }
    ]
},
```

Importantly, the `merchantTermsAndConditionsType` must be `1`.

**How to verify**: 

- Ensure you have successfully booked for a product with a 'standard' cancellation policy by verifying that its booking status is `"CONFIRMED"`.
- Ensure that you have cancelled this booking

**To pass the test**: Successfully cancelling a confirmed booking of any product with a standard cancellation policy is sufficient to pass this test.

### Check 8: A booking with an 'all sales final' cancellation policy has been successfully confirmed and cancelled

This test looks to see if a product with an 'all sales final' cancellation policy has been successfully confirmed and then cancelled by you. Products in this category cannot be cancelled or amended without incurring a 100% penalty; i.e., the refund amount will be zero.

Products with an 'all sales final' cancellation policy will return a `merchantTermsAndConditionsType` of `3`.

**How to verify**: 

- Ensure you have successfully booked for a product with an 'all sales final' cancellation policy by verifying that its booking status is `"CONFIRMED"`.
- Ensure that you have cancelled this booking

**To pass the test**: Successfully cancelling a confirmed booking of any product with an 'all sales final' cancellation policy is sufficient to pass this test.

### Check 9: An 'on-request' product has been booked and confirmed

**Note**: This check is only for partners that have access to Viator's on-request product catalogue, which is not granted by default as it requires a slightly different workflow. See: [Selling on-request products](../../../papiv1/key-concepts/selling-on-request-products) for more information on how to gain access to this section of the catalogue.

**To pass the test**: 

Make a test booking in the sandbox environment for an on-request product. You can determine if a product is an on-request product according to the `bookingEngineId` field, which will have a value of `DeferredCRMBE`. Or, use one of the following on-request products:

- 100213P25
- 100245P200

Make a booking for the product, ensuring that `demo` is set to `false`.

If successful, the booking status in the response will have a `type` of `PENDING`.

```javascript
"bookingStatus": {
  "status": 4,
  "text": "Pending",
  "type": "PENDING",
  "level": "ITINERARY",
  "confirmed": false,
  "pending": true,
  "amended": false,
  "cancelled": false,
  "failed": false
}
```

[Send an email to the Application Support team](mailto:apitechsupport@viator.com) and ask for this booking to be confirmed for you, mentioning this is for site-certification purposes.

### Check 10: A booking with a passenger mix containing multiple age bands has been made

This test ensures that your implementation supports bookings for multiple passengers in different age categories (age bands); i.e., `Adult`, `Child`, `Senior`, `Youth` and `Infant`.

**To pass the test**: Make a test booking for a product, including at least two passengers, each from different age bands. The product must support multiple age bands; e.g., `5010SYDNEY`

**How to verify:** Inspect the response from [/booking/book] when you make the booking and ensure that the `travellerAgeBands` array contains more than one item; e.g.:

```javascript
"travellerAgeBands": [
  {
    "sortOrder": 0,
    "count": 1,
    "pluralDescription": "Adults",
    "description": "Adult",
    "ageBandId": 1
  },
  {
    "sortOrder": 1,
    "count": 1,
    "pluralDescription": "Children",
    "description": "Child",
    "ageBandId": 2
  }
]
```