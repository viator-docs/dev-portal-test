# Add to cart

## Summary

Because the Viator API is both stateless and does not include services for managing a customer's shopping cart, this functionality must be fully managed on the merchant's side.

Viator's servers can generate a price quote for one or several items, but this collection will not be saved by the server.

We recommend the following process:

1. **View product** - allow the customer to browse products and product details
2. **Select date and passengers** - allow the customer to check available dates and select the type and number of passengers
3. **Select tourgrade** - display the available tour grades
4. **View cart** -> **price quote** - display a total price (including multiple items)

## View product
From your *view product details* section, a button should be presented to **check availability** or **book now** or something similar. Use the [/product](../../../../openapi/reference/operation/product) service to retrieve product details and schedules.

## Select date and passengers
Here, the customer is presented with the dates that remain available for the product. Available dates can be obtained via a request to [/booking/availability/dates](../../../../openapi/reference/operation/bookingAvailabilityDates). The list of [age bands](../../key-concepts/working-with-age-bands) (i.e.: 'adult', 'child'; and, each band's maximum and minimum age) is available from the [/product](../../../../openapi/reference/operation/product) service.

**Validation** – ensure that:
1. The total number of travelers being booked does not exceed the limit returned in the `maxTravellerCount` field of the [/product](../../../../openapi/reference/operation/product) service
2. *At least one* adult (or adult equivalent) passenger has been selected
3. The selected dates are available (use [/booking/availability/dates](../../../../openapi/reference/operation/bookingAvailabilityDates))

## Select tour grade

Once the date and traveler mix have been selected, the customer may need to select a tour grade. Products that do not have this information recorded in the Viator database are assigned a tour grade of `"DEFAULT"`. For these products, it is unnecessary to select a tour grade.

Tour grade options must be displayed to the customer when the product has multiple tour grades, due to:
- different departure times
- traveler mix price deals, like family passes
- different inclusions and prices - e.g., limo pickup, different language delivery options), or...
- non-default tour grades – these tour grades *must* be presented to the customer.

The tour grades for a product are retrieved with the [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades) service. If a tour grade is available (check the `available` flag), an 'add to cart' button can be displayed. For unavailable tour grades (`available` is `false`), a reason (`unavailableReason`) is provided, which will be one of:

- `"TRAVELLER_MISMATCH"`
- `"BLOCKED_OUT"`, or
- `"UNAVAILABLE"`

You may choose to hide the blocked-out tour grades (if all tour grades are unavailable on a particular day, a day's tour grades are all `"BLOCKED_OUT"`, or the product is not operating that day, the [/booking/availability/dates](../../../../openapi/reference/operation/bookingAvailabilityDates) service will not return that date).

You may also find that a product has no bookable tour grades on a day if the traveler mix does not meet that tour grade's traveler mix restrictions. For example, a honeymoon might require two adults as the only possible traveler mix.

You can choose to hide the `"BLOCKED_OUT"` tour grades, but you will probably want to display the `"TRAVELLER_MISMATCH"` grades with the traveler mix requirements listed so that the customer can elect to alter their traveler mix to suit (e.g., for family passes, etc.) You cannot allow the passenger to book with an incompatible traveler mix.

The return object includes information about tour grade restrictions:

`ageBandsRequired`:
- `minimumCountRequired`: minimum number of travelers for an [age band](../../key-concepts/working-with-age-bands)
- `maximumCountRequired`: maximum number of travelers for an [age band](../../key-concepts/working-with-age-bands). If this field is `null`, any number of travelers may be booked.

You will need to present the `ageBandsRequired` information as in the following examples:

| In 'Adult' age band | Meaning |
|---------------------|---------|
| `minimumCountRequired` = 1<br>`maximumCountRequired` = 3 | From 1 to 3 adults |
| `minimumCountRequired` = 0<br>`maximumCountRequired` = 3 | up to three adults |
| `minimumCountRequired` = 3<br>`MaximumCountRequired` =  | 3 or more adults |
| `minimumCountRequired` = 2<br>`MaximumCountRequired` = 2 | 2 adults |


**Note:** There is a singular and plural version for each age band definition. We recommend automatically generating language that ensures the samples above are grammatically correct using this information.

The user can click **back** and change their traveler mix; or, they can try selecting another day.

## View cart / price quote

Once the item has been added to the cart, you will need to preserve this information as part of the session data in your back-end, or as a browser cookie. In particular, you will need to store the *product code* and *age band to quantity traveler mix*.

### Sample PHP shopping cart item class
**Note:** this is only an example and is implementation dependent. Alternatively, you could use the item class (as used in the API calls), but much of the information it contains does not need to be stored:

```javascript
class shoppingCartItem {
  var $productCode;
  var $tourGrade;
  var $date;
  var $ageBandIdToQty; // assoc array
}
```

To obtain a price quote, you must call the [/booking/calculateprice](../../../../openapi/reference/operation/bookingCalculateprice) service. This service requires a currency code and an array of items to be specified. Each item contains the date, product code, tour grade code and an associative array of `ageBandIds` -> `quantity`.

**Validation** – the data must contain [valid age bands](../../key-concepts/working-with-age-bands), tour codes, dates, etc. acquired in previous requests to the API.

**Note:** A product's availability can change in the time between API calls as tour grades or products may be blocked out, or the booking window may close for upcoming dates.