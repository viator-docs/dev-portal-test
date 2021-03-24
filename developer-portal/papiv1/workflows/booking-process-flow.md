## Booking process flow

In this section, we show a sample booking process flow using Viator API services.

### Search for a product

1. Determine the `destinationId` for the desired destination using the [/taxonomy/destinations](../../../../../openapi/reference/operation/taxonomyDestinations) service.
2. Search for products in the destination with the [/search/products](../../../../openapi/reference/operation/searchProducts) service using the `destinationId`, along with optional parameters like the date range (`startDate` and `endDate`), attraction link (`seoId`), category (`catId`) and subcategory (`subCatId`).

#### Example [/search/products](../../../../openapi/reference/operation/searchProducts) POST request body:

```javascript
{
  "startDate": "2018-12-25",
  "endDate": "2018-12-28",
  "topX": "1-15", 
  "destId": 684, 
  "currencyCode": "USD",
  "sortOrder": "TOP_SELLERS"
}
```
**Note**: the `startDate` and `endDate` must be in the future.

3. Get the product details with the [/product](../../../../openapi/reference/operation/product) service.

#### Example parameters for a [/product](../../../../openapi/reference/operation/product) GET request:

```html
code=5010SYDNEY&currencyCode=USD
```

### Determine the product's available dates

Use the [/booking/availability/dates](../../../../openapi/reference/operation/bookingAvailabilityDates) service to retrieve a list of dates on which the product is operating. This list can be used to populate a calendar display of available dates.

#### Example parameters for an availability check using the [/booking/availability/dates](../../../../openapi/reference/operation/bookingAvailabilityDates) service:

```html
productCode=5010SYDNEY
```

### Determine the available age bands for the product

Because the product option (tour grade) availability check requires the desired passenger mix, the user will first need to select the number of travelers and the age band into which each can be classified.

Note: The exact ages to which each age band refers to differs between products. See [Working with age bands](../key-concepts/working-with-age-bands) for more information.

1. Determine the number of passengers/travelers (and their respective agebands) by having the user input the passenger mix for which they wish to make a booking.
2. Check for available tour grades for the date chosen using the [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades) service; or, check for available tour grades by month using the [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) service

#### [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades) POST request:
```javascript
{
  "productCode": "5010SYDNEY",
  "bookingDate": "2018-12-05",
  "currencyCode": "USD",
  "ageBands": [
    {      
      "bandId": 1,
      "count": 2    
    }  
  ]
}
```

#### [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgrades) POST request:
```javascript
{
  "productCode": "5010SYDNEY",
  "month": "12",
  "year": "2018",
  "currencyCode": "USD"
}
```
3. Finalize pricing using the [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) service. 
  - **Note**: we strongly recommend using the [/booking/calculateprice](../../../../openapi/reference/operation/bookingCalculateprice) service prior to making the booking, as it reconfirms the product availability for the specified dates and passenger mix.

4. Make the booking
  - Note that the [/booking/book](../../../../openapi/reference/operation/bookingBook) service supports multi-item bookings. The response from the [/product](../../../../openapi/reference/operation/product) service indicates mandatory information that must be sent when making the booking, such as required [booking questions](../appendices/booking-questions) and hotel pick-up options.
  - For hotel pick-up:
    - Send a hotel ID to the [/booking/book](../../../../openapi/reference/operation/bookingBook) service if `hotelPickup` is `true` for the product.
    - [/booking/hotels](../../../../openapi/reference/operation/bookingHotels) can be used to return a list of hotels available for the product.
    - The `hotelId` is the `id` field in the response from the [/booking/hotels](../../../../openapi/reference/operation/bookingHotels) service. This can be:
      -  a number (represented as a string) – e.g., `'4119'`
      -  `'local'` – if the customer resides near the location in which the product operates 
      -  `'notBooked'` – if the customer's hotel is not yet booked
      -  `'notListed'` – if the customer's hotel is not listed in the response from [/booking/hotels](../../../../openapi/reference/operation/bookingHotels). If this is the case, capture the customer’s hotel details in a text box and pass this information in the `pickupPoint` field in the request body of the [/booking/book](../../../../openapi/reference/operation/bookingBook) service.

#### [/booking/book](../../../../openapi/reference/operation/bookingBook) POST request example:

```javascript
{
  "demo": true,
  "currencyCode": "USD",
  "partnerDetail": {
      "distributorRef": "distributorRef1550616101308"
  },
  "booker": {
      "firstname": "Homer Test",
      "surname": "Simpson Test",
      "title": "Mr",
      "email": "apitest@viator.com",
      "homePhone": "(02)66987564"
  },
  "items": [
    {
      "partnerItemDetail": {
        "distributorItemRef": "distributorItemRef1550616101308"
      },
      "hotelId": null,
      "pickupPoint": null,
      "travelDate": "2019-03-19",
      "productCode": "5010SYDNEY",
      "tourGradeCode": "24HOUR",
      "languageOptionCode": "en/SERVICE_GUIDE",
      "bookingQuestionAnswers": [
        {
          "questionId": 100,
          "answer": "120 kgs"
        }
      ],
      "specialRequirements": "",
      "travellers": [
        {
          "bandId":1,
          "firstname": "Homer",
          "surname": "Simpson Test",
          "title": "Mr",
          "leadTraveller": true
        }, {
          "bandId": 1,
          "firstname": "Marge",
          "surname": "Merchant Viator Test",
          "title": "Mrs"
        }
      ]
    }
  ]
}
```

#### [/booking/hotels](../../../../openapi/reference/operation/bookingHotels) GET example parameters:

```html
productCode=5010SYDNEY
```
or
```html
destId=684
```

5. You will receive different booking statuses depending on the product's booking engine. 
  - For products that are free-sale (`'FreesaleBE'`), unconditional free-sale (`'UnconditionalBE'`) and free-sale / on-request (`'FreesaleOnRequestBE'`) - i.e., during the free-sale period, confirmation should occur instantly. 
  - The `'FreesaleOnRequestBE'` status means that the product will only remain free-sale up until a certain number of days before the travel date, after which it becomes on-request. 
  - Normally, if the product is on-request, its booking status will be `'Pending'`.

### Post-booking

#### Displaying vouchers

To display the voucher to your customer, direct them to the URL returned in the `voucherURL` field in the response from the [/booking/book](../../../../openapi/reference/operation/bookingBook) service.

**Note**: the voucher will not be available until the booking is confirmed – the value of the `hoursConfirmed` field in the response from the [/booking/book](../../../../openapi/reference/operation/bookingBook) service can be shown to the customer to indicate the time frame within which they are likely to be notified as to their booking confirmation.

#### Viewing bookings

- To view a **single** booking, use the [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking) service, which returns one booking at a time. 
- To retrieve **all** bookings for a customer, use the [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) service.

#### Viewing booking statuses

To view the booking statuses for multiple items based on various criteria, use the [/booking/status](../../../../openapi/reference/operation/bookingStatus) service.

Note that this service can only be polled once every five minutes. Ideally, this service should be used by your software implementation to perform bulk updates of pending itineraries. The maximum number of itinerary results returned is 1,000.

#### `merchantNetPrice` and `price` in the [/search/products](../../../../openapi/reference/operation/searchProducts) service

The difference between these fields is as follows:

- `merchantNetPrice` is the amount you, as the merchant partner will be invoiced for, excluding any fees.
- `price` is the price at which Viator sells the product

**Note**: these prices are also returned (per age band) by the following services:

- [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix)
- [/booking/pricingmatrix](../../../../openapi/reference/operation/bookingPricingmatrix)
- [/booking/book](../../../../openapi/reference/operation/bookingBook)

### Currency considerations for bookings

If the booking shows prices converted to and formatted according to a different currency to that in which it was made, it is because each API partner has a particular 'base currency', and all bookings will be made in that currency.

### Booking and transaction fees

The total price may be different to the `merchantNetPrice` due to the fact that a booking fee – i.e., a transaction fee or commission – is added to all bookings.

This fee is a fixed percentage with a capped maximum. The exact percentage depends on your merchant partner agreement with Viator and can be found in your contract with Viator.

### Making demo bookings

To make a demo booking, simply set the `demo` field to `true` in the [/booking/book](../../../../openapi/reference/operation/bookingBook) service.

While demo bookings are allowed on the live production environment, we recommend not doing so as it is possible that a notification could be sent to the supplier. Performing a cancellation of the demo booking is therefore recommended.

To cancel the booking, partners should use the [/bookings/{id}/cancel](../../../../openapi/reference/operation/cancelBooking) service. For details on canceling a booking, see: [Cancellation API workflow](../common-workflows-and-data-validation/cancellation-api-workflow)