# Checkout

## Summary

The checkout process can be accomplished using the [/booking/book](../../../../openapi/reference/operation/bookingBook) service, but you may need to make requests to other services to calculate prices, display product information (age-band names, etc.) and list available pick-up hotels for user selection.

The Viator mobile website breaks the workflow down into three steps. For multiple items, some steps will need to be repeated, such as capturing the traveler names for each tour. In your implementation, you can reorganize/reorder the data collection to better suit your needs.

**Example workflow:**

1. **Collect traveler details** - collect the names of the lead traveler and all other travelers
2. **Collect travel details** - ask any additional questions, including those about any special requirements, and select pick-up options.
  - **Note:** the aforementioned steps will need to be repeated for each item)
3. **Collect booking details**
  - **Note:** billing and payment details *should not* be sent to Viator.

The classes for the booking request are defined here in Booking Data Classes. You will need implement these in your chosen programming language and verify that the correct JSON objects are generated during serialization.

## Traveler details

The traveler details are used to populate the `booking->items[]->travellers[]` objects.

One passenger must be identified as the "lead traveler". A boolean field in the traveler object represents this flag. The lead traveler must be an adult or have the age band `treatAsAdult` flag set to `true`.

All other travelers must be included.

**Validation** 

Ensure that:

  - `bandId` is a [valid age band ID](../../key-concepts/working-with-age-bands) for the product
  - `firstname` is less than 16 chars
  - `surname` is less than 36 chars
  - `title` is not included unnecessarily (it is optional)
  - `leadTraveller` is set to `true` for one of the travelers who is in an [age band](../../key-concepts/working-with-age-bands) that has the `treatAsAdult` flag set to `true`

## Other details

### Booking questions

The travel details include the [booking questions](../../appendices/#booking-questions) that are supplied in the [/product](../../../../openapi/reference/operation/product) service.

**Sample question**

**Note:** There may be more than one.

```javascript
bookingQuestions: [{
  "message": "For safety reasons you must enter the weight of <b>all</b> passengers",
  "required": true,
  "questionId": 23,
  "title": "Passenger Weights",
  "subTitle": "(e.g. 127 pounds, 145 kilos, etc)",
  "sortOrder": 1
}]
```
The questions should be displayed with the title, message, subtitle and whether it is mandatory (required) or not.

**Validation** – if the question is mandatory, the user must enter at least one character.

### Special requirements

'Special requirements' should be presented as a text input field so that customers can record whether they require wheelchair assistance, for example. It is not mandatory for the customer to enter any text.

### Pick-up information

The last thing that must be collected for each item being booked is the pick-up information. If the product includes pick-up, the `hotelPickup` flag will be set to `true` (in the product object).

If pick-up is included, you will need to make a request to [/booking/hotels](../../../../openapi/reference/operation/bookingHotels) to determine if a hotel list exists. If it does, the list must be displayed so that the customer can make their selection. If not, a text input field should be displayed for hotel name collection.

Please note that the *first three results* in the list are *not* hotels; rather, these three are alternative selections, comprising:

- 'Hotel not listed'
- 'Live locally or staying with family/friends'
- 'Hotel not yet booked'

If the customer selects 'hotel not listed', you *must* provide a hotel selection text input field. For the other two options, no hotel name is required. In all cases, the hotel ID must be updated with either a hotel ID or the IDs of the three items listed.

If no hotel list is available, you must provide a text input field for collecting the customer's hotel name. Please include instructions to enter 'live locally' or 'hotel not yet booked' if they cannot provide a hotel name.

**Validation** – if hotel list is available, the `hotelId` must be supplied. If the `hotelId` is `notListed` the, `pickupPoint` field *must* have at least one character.

If a hotel list is not available, then `pickupPoint` must contain a value.

### Language services

If the response from the [/product](../../../../openapi/reference/operation/product) service contains information in the `languageServices` field, e.g.:

```javascript
"langServices": {
  "en/SERVICE_AUDIO": "English - Audio"
}
```

...you must specify which language option you wish to book for this tour in the `languageOptionCode` field (see request body schema of the [/booking/book service](../../../../openapi/reference/operation/bookingBook)).