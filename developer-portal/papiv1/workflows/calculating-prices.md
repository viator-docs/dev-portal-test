# Calculating prices

The [/booking/calculateprice](../../../../../openapi/reference/operation/bookingCalculateprice) service is used to calculate a total price for one or more products, with the ability to specify the date and passenger mix for each product individually. 

It also reconfirms the availability and pricing of the products for the requested dates and passenger mixes. We strongly recommended that you call this service prior to [making a booking](../making-a-booking) to establish that the booking will succeed once submitted.

This endpoint is useful when implementing a shopping cart, as multiple product bookings can be enquired about in a single call to this service.

### Example request body (JSON)

```javascript
{
  "currencyCode": "USD",
  "items": [{
    "travelDate": "2015-03-01",
    "productCode": "2916ROME",
    "tourGradeCode":"24HR",
    "travellers": [
      {
        "bandId": 1
      },
      {
        "bandId": 1
      }
    ]
  }]
}
```
The quirky `travellers` array is used to specify the number of travellers in each age band. Each member object of this array corresponds to a single traveller. The example above signifies "two adults from bandId: 1".

Another example might be "three travelers from bandId:1 and two travelers from bandId:2". That would be as follows:

```javascript
"travellers": [
  {
    "bandId": 1
  },
  {
    "bandId": 1
  },
  {
    "bandId": 1
  },
  {
    "bandId": 2
  },
  {
    "bandId": 2
  }
]
```

- **Note**: See [Working with age bands](../../key-concepts/working-with-age-bands) for more information.

### Price information

The **total price** (i.e., including the transaction fee) that you will be invoiced for the products to be booked is given in the following fields in the response from this service:

- `data.itemSummaries[].price` (numeric total price of item)
- `data.itemSummaries[].priceFormatted` (currency formatted total price of item) 
- `data.itinerary.totalPrice` (numeric total price of item)
- `data.itinerary.totalPriceFormatted` (currency-formatted total price of item)

For more information about pricing fields and their meaning throughout this API, see: [Merchant pricing](../../key-concepts/merchant-pricing).

### Determining whether the product is 'freesale' or 'on request'

You can determine whether the booking is *freesale* or *on-request* by examining the response from this endpoint. 'Freesale' bookings are those that are confirmed immediately (with a status of `"CONFIRMED"`) when booked, while *on-request* bookings are instead confirmed by the supplier at a later time. 

The approximate time window for confirmation is provided in the `hoursConfirmed` (integer) field. This can be presented to the customer. 

- An `hoursConfirmed` of `0` means that the booking is *freesale*. 
- An `hoursConfirmed` greater than `0` indicates that the booking is *on-request*.