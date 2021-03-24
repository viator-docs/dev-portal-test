# Making a booking

To make a booking, use the [/booking/book](../../../../openapi/reference/operation/bookingBook) service.

To make a *real* booking, ensure `demo` is set to `false` in the booking request.

Demo bookings will enter our system as a test booking and will not charge the merchant. To enable demo bookings, set `demo` to `true` in the booking request and pass `"test"` as the traveler's first or last name.

**Note:** Avoid testing on **Live**, as the booking may be sent to the supplier (depending on the product). Any test bookings on live must be cancelled via the [/bookings/{id}/cancel](../../../../openapi/reference/operation/cancelBooking) service; or, contact dpsupport@viator.com if you experience any other issues.

## distrbutorRef and distributorItemRef

The `distributorRef` and `distrbutorItemRef` fields are the merchant partner's own reference for the booking. All merchant partners must pass a `distributorRef` and a `distributorItemRef` in all bookings.

It can be any alphanumeric string, and in for the `distrbutorRef`, it must be unique to bookings made by the merchant.

If an existing `distributorRef` is passed, the booking with the matching `distributorRef` will be returned in the response, and a new booking will not be made.

Please see the description for these fields in the table below for more information.

**Example request**

```javascript
{
  "demo": true,
  "currencyCode": "USD",
  "partnerDetail": {
    "distributorRef": "distroRef0412141435"
  },
  "booker": {
    "email": "apitest@viator.com",
    "firstname": "Homer Test",
    "surname": "Simpson Test",
    "title": "Mr"
  },
  "items": [{
    "partnerItemDetail": {
      "distributorItemRef": "distroItemRef0412141435_1"
    },
    "hotelId": null,
    "pickupPoint": null,
    "travelDate": "2015-03-31",
    "productCode": "2916ROME",
    "tourGradeCode": "24HR",
    "languageOptionCode": "en/SERVICE_GUIDE",
    "bookingQuestionAnswers": [{
      "questionId": 100,
      "answer": "120 kgs"
    }],
    "specialRequirements": "",
    "travellers": [{
      "bandId": 1,
      "firstname": "Homer",
      "surname": "Simpson Test",
      "title": "Mr",
      "leadTraveller": true
    }]
  }]
}
```

**Description of JSON request parameters**

| Object name | Element name | Type | Comments | Mandatory |
|-------------|--------------|:----:|----------|:---------:|
|        | `demo`      | boolean | If this is set to True, then it is a demo booking only. Full demos do not send any notifications, are automatically confirmed and OnRequest products become freesale products. Default value is true. Production must have `demo` set to `false`. | ❌ |
|        | `currencyCode` | string | The currency the booking will be submitted in. You will be billed in this currency. | ❌ |
| `partnerDetail` | | object | Applicable only for extra partner detail for either partner or merchant partner for sending partner specific information | ❌ |
|  | `distributorRef` | string | Merchant API partners must pass a `distributorRef` at itinerary level in the `partnerDetails` object. The `distributorRef` passed must be alphanumeric and unique to bookings made by the merchant. Passing an existing `distributorRef`: If an existing distributorRef is passed, the booking with the matching `distributorRef` will be returned in the response and a new booking will not be made. The fields in the response are identical to the response for a new booking.| ✅ |
| `partnerItemDetail` |  | object | For extra partner detail at an item level, for either partner or merchant partner. | ❌ |
|  | `distributorItemRef` | string | Merchant API partners must pass a `distributorItemRef` into the `partnerItemDetails` object for each item in the items object. The `distributorItemRef` passed must be alphanumeric and unique to the itinerary. | ✅ |
| `booker` |  | object | The information of the primary contact. This contact does not have to be a traveler. | ✅ |
|  | `email` | string | Email address of the primary contact | ❌ |
|  | `homePhone` | string | Home phone number of the primary contact | ❌ |
|  | `firstname` | string | First name of the primary contact | ✅ |
|  | `surname` | string | Surname of the primary contact | ✅ |
|  | `title` | string | Title of the primary contact | ❌ |
| `items` |  |  | Array of items in itinerary to be booked | ✅ |
|  | `productCode` | string | product code of the itinerary to be booked | ✅ |
|  | `tourGradeCode` | string | `tourGradeCode` of the item to be booked. If tour grades are supplied in [/product](../../../../openapi/reference/operation/product), you must allow the customer to select a tour grade code. If no tour grades are available for the product, pass `"DEFAULT"`. | ✅ |
|  | `languageOptionCode` | string | The language service provided for this product that has been chosen for this booking. Usually in the format langcode/Service eg `"en/SERVICE_GUIDE"`. If the product details service [/product](../../../../openapi/reference/operation/product) for the product returns a langService, this must be provided. | ✅<br />(if `languageServices` are provided in [/product](../../../../openapi/reference/operation/product)) |
|  | `travelDate` | date | date of travel for the item (format is YYYY-MM-DD; e.g. 2013-12-25) | ✅ |
|  | `hotelId` | string | If [/product](../../../../openapi/reference/operation/product) returns `hotelPickup`: `true` and a list of hotels is available for this product in [/booking/hotels](../../../../openapi/reference/operation/bookingHotels), a `hotelId` must be captured. The hotel id as per the hotel service (id field) or use one of these alternative hotel ids:<br />`local`: customer lives locally<br />`notBooked`:  Customer has not booked their hotel yet<br />`notListed`: Hotel not listed | ✅<br />(if [/product](../../../../openapi/reference/operation/product) returns `hotelPickup`: `true` for `productCode` and hotels available) |
|  | `pickupPoint` | string | Pickup point information related to hotel pickup. Details of the hotel or pickup point must be provided if the `hotelId` selected by the user is `"notListed"` or if no hotels are returned for the product in [/booking/hotels](../../../../openapi/reference/operation/bookingHotels) where `hotelPickup`: `true` | ✅<br />(if `hotelId` = `"notListed"` or no hotels returned) |
|  | `specialRequirements` | string | Capture any additional requirements for the booking, such as dietary requirements or if a wheelchair is required. Suggested workflow is if there are no `bookingQuestionAnswers` for the product, to collect `specialRequirements`. | ❌ |
| `travellers` |  |  | Array of traveller names with a required lead traveller selected per item. | ✅ |
|  | `bandId` | integer |  [Age band id](../../key-concepts/working-with-age-bands). Available age band details for the product is listed in [/product](../../../../openapi/reference/operation/product). | ✅ |
|  | `firstname` | string | First name of the traveller | ✅ |
|  | `surname` | string | Surname of the traveller | ✅ |
|  | `title` | string | Title of the traveller. Suggested options: Mr, Mrs, Ms, Miss, Mstr, Dr | ✅ |
|  | `leadTraveller` | boolean | Each item must have one traveller assigned as the lead traveller for the tour. The lead traveller will have a value of true, all other travellers will have a value of false. The lead traveller can have a mobile phone number added to the booking. | ✅ |
|  | `cellPhoneCountryCode` | string | Ideally only collect the phone number country code for the lead traveller. Alternatively, collect the phone number of the booker instead. | ❌ |
|  | `cellPhone` | string | Ideally only collect the phone number country code for the lead traveller. Alternatively, collect the phone number of the booker instead. | ❌ |
| `bookingQuestionAnswers` |  | object | Answers to [booking questions](../../appendices/#booking-questions) for the particular item. If a booking question is available in the `bookingQuestions` array in [/product](../../../../openapi/reference/operation/product) for the product, the matching `bookingQuestionAnswers` must be passed. If a product does not have any `bookingQuestion` items, you can omit the `bookingQuestionAnswers` field completely. Any invalid or unnecessary `bookingQuestionAnswers` that are passed to [/booking/book](../../../../openapi/reference/operation/bookingBook) will be ignored (no exceptions will be raised) | ✅<br />(if [/product](../../../../openapi/reference/operation/product) returns `bookingQuestions`) |
|  | `questionId` | integer | `questionId` (provided in [/product](../../../../openapi/reference/operation/product)) | ✅ |
|  | `answer` | string | Answer to the question at the `questionId` listed. Recommended length for the answer is 500 characters. | ✅ |

**JSON Response**

The prices returned in the booking response represent the net rate, which is the amount merchant API partners will be invoiced for. See [Merchant pricing](../../key-concepts/merchant-pricing) for more information.

## Booking errors

A number of errors may be returned in the response to the [/booking/book](../../../../openapi/reference/operation/bookingBook) service. If any errors are reported, NO items will be booked, NO items/data will be returned in the response, and NO billing will occur.

There are two error types:
- `"VALIDATION"` - occurs if a required field is missing or a field is not formatted properly
- `"EXCEPTION"` - occurs when there are issues with the booking date, product / tour grade code or the payment.

Example of an error message:

```javascript
{
  "data": null,
  "vmid": "221001",
  "errorMessage": [ "Missing distributor reference" ],
  "errorType": "EXCEPTION",
  "dateStamp": "2013-04-24T15:50:05+0000",
  "errorReference": "~3713624959841553334512668",
  "errorMessageText": ["Missing distributor reference" ],
  "success": false,
  "totalCount": 1,
  "errorName": "Exception"
}
```

Please see "Standard JSON fields" in the Appendix for an explanation of the fields.

| Scenario | `errorType` | Example error message text |
|----------|-----------|----------------------------|
| Lead traveller is not specified | `VALIDATION` | A traveler needs to be selected as lead traveler. Lead Traveler's name must match credit card name. |
| Traveller names are not provided | `VALIDATION` | First name of traveler 1 is required, Last name of traveler 1 is required |
| No travellers provided | `VALIDATION` | A traveler needs to be selected as lead traveler. Lead Traveler's name must match credit card name. |
| Product code does not exist | `EXCEPTION` | We're sorry, we cannot find the tour, activity or attraction you are looking for
| Product code exists, but tour grade code does not exist | `EXCEPTION` | SICInvalidTourGrade |
| Booking date is in the past | `EXCEPTION` | We're sorry, the following tour you are trying to book is sold out and no longer available: Grand Canyon All American Helicopter Tour (2280AAHT) |
| `languageOptionCode` is in the wrong format | `EXCEPTION` | languageOptionCode should be LangCode/LangServices |
| Missing required answers for item | `EXCEPTION` | Additional questions missing |
| Unsupported currency | `EXCEPTION` | Could not lookup SGD:java.lang.RuntimeException: Could not lookup SGDf:au.com.fim.v3.etravel.PiusRecordNotFoundException: No currency found: select * from CurrencyFormat where currencyID = 'SGD' |
| `distributorRef` not provided in `partnerDetail` object | `EXCEPTION` | Missing distributor reference |
| `distributorItemRef` not provided in `partnerItemDetail` object | `EXCEPTION` | Missing distributor item reference |
| `partnerItemDetail` object not provided for the item | `EXCEPTION` | Missing partner item details! |