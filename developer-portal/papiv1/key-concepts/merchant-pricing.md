# Merchant pricing

Many of the endpoints in the Viator API return pricing information. Due to the necessity of supporting legacy implementations, some pricing fields may not be named in an intuitive way.

This section seeks to clarify what each of the pricing fields returned by each endpoint actually refer to and how you need to use this information in your implementation.

## Categories of pricing

| Category | Meaning |
|----------|---------|
| **Suggested retail price** | The recommended retail price for the product and the price that the product is sold at on the Viator site |
| **Merchant net rate** | The amount that Viator will invoice the merchant for this sale, **excluding the transaction fee** |
| **Merchant total price** | The total amount that Viator will invoice the merchant for this sale, **including the transaction fee** |

| Service | Suggested retail price | Merchant net rate |
|---------|------------------------|-------------------|
| [/search/products](../../../../openapi/reference/operation/searchProducts) | `price`, `priceFormatted` | `merchantNetPriceFrom`, `merchantNetPriceFromFormatted` |
| [/product](../../../../openapi/reference/operation/product) | `price`, `priceFormatted`, `priceFrom`, `priceFromFormatted` | `merchantNetPriceFrom`, `merchantNetPriceFromFormatted` |
| [/booking/availability](../../../../openapi/reference/operation/bookingAvailability) | `retailPrice`, `retailPriceFormatted` | `merchantNetPrice`, `merchantNetPriceFormatted` |
| [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades) | `retailPrice`, `retailPriceFormatted` | `merchantNetPrice`, `merchantNetPriceFormatted` |
| [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) | `price`, `priceFormatted` | `merchantNetPrice`, `merchantNetPriceFormatted` |
| [/booking/book](../../../../openapi/reference/operation/bookingBook) | N/A | `merchantNetPrice`, `merchantNetPriceFormatted`, `lastRetailPrice`, `lastRetailPriceFormatted` |
| [/booking/pricingmatrix](../../../../openapi/reference/operation/bookingPricingmatrix) | `price`, `priceFormatted` | `merchantNetPrice`, `merchantNetPriceFormatted` |
| [/booking/calculateprice](../../../../openapi/reference/operation/bookingCalculateprice) | N/A | `merchantNetPrice`, `merchantNetPriceFormatted`, `lastRetailPrice`, `lastRetailPriceFormatted`, `itineraryFromPrice`, `itineraryFromPriceFormatted`, `itineraryNewPrice`, `itineraryNewPriceFormatted` |
| [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking) | N/A | `merchantNetPrice`, `merchantNetPriceFormatted`, `lastRetailPrice`, `lastRetailPriceFormatted`,  |
| [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) | N/A | `merchantNetPrice`, `merchantNetPriceFormatted`, `lastRetailPrice`, `lastRetailPriceFormatted` |

The following services return the **merchant total price** (i.e., the merchant net rate + transaction fee) in the fields shown:

| Service | Merchant total price fields |
|---------|-----------------------------|
| [/booking/book](../../../../openapi/reference/operation//bookingBook) | `price`, `priceFormatted`, `totalPrice`, `totalPriceFormatted`, `priceUSD`, `totalPriceUSD` |
| [/booking/calculateprice](../../../../openapi/reference/operation/bookingCalculateprice) | `price`, `priceFormatted`, `priceUSD`, `totalPrice`, `totalPriceFormatted`, `totalPriceUSD` |
| [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking) | `price`, `priceFormatted`, `totalPrice`, `totalPriceFormatted`, `priceUSD`, `totalPriceUSD`, |
| [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) | `price`, `priceFormatted`, `priceUSD`, `totalPrice`, `totalPriceFormatted`, `totalPriceUSD` |

## Using the pricing fields

Let's have a look at the various pricing fields for a specific product - the Grand Canyon All-American Helicopter Tour (product code: 2280AAHT).

A search for this product (at the time of writing) on the Viator.com site gives the 'from' price of the tour as **$601.11**:

<figure>
    <img src="../../images/guide/suggested-retail-price.jpg" alt="'from' price on viator.com product detail page"/>
    <figcaption>Retail price shown on viator.com</figcaption>
</figure>

This value comes from the `price` or `priceFormatted` fields in the response from [/product](../../../../openapi/reference/operation/product) for this tour; i.e.,:

```javascript
{
  ...,
  "price": 601.11,
  "priceFormatted": "$601.11",
  ...
}
```

Essentially, this value is the price that the product is sold at on the viator site, and is therefore the **suggested retail price**; i.e., the price at which we recommend you advertise and sell the product for.

It is also the price of the tour grade with the lowest price; in this case, the "Earlybird A-Star" tour grade detailed here:

```javascript
{
    "sortOrder": 5,
    "currencyCode": "AUD",
    "langServices": {
        "en/SERVICE_AUDIO": "English - Audio"
    },
    "gradeCode": "EB_ASTAR_SP",
    "merchantNetPriceFrom": 555.37,
    "priceFrom": 601.11,
    "priceFromFormatted": "$601.11",
    "merchantNetPriceFromFormatted": "$555.37",
    "gradeTitle": "Special: Earlybird A-Star",
    "gradeDepartureTime": "",
    "gradeDescription": "Special Offer: Receive a discounted seat for the Grand Canyon All American Helicopter Tour departing between 6:45am and 7am on an A-Star helicopter",
    "defaultLanguageCode": "en"
},
```

As you can see, the "suggested retail price" is given in the `priceFrom` field.

### Low and zero-margin products

When setting the retail price at which you sell products on your site, it’s important to remember that the “suggested sell price” is the price at which the product is currently advertised on the Viator site and reflects the current standard industry price for the product. It <u>does not</u> take into consideration the merchant net price (i.e., the price at which you as a merchant partner will be invoiced for the sale) and, in the case of discounting scenarios, may in fact be less than the merchant net price.

Therefore, to ensure that you sell the product at a price which guarantees that you receive at least as much as you will be invoiced for, as well as any extra profit margin that you desire to generate, we recommend you include a check in your implementation that these requirements are satisfied by comparing the suggested retail and merchant net prices and adjusting the retail price at which you advertise the product accordingly.

For example, if the merchant net price for a product is $100, the suggested sell price is $101, and your requirement for a minimum margin is 5%, you should adjust the price at which you advertise the product to $105.

While we recommend that you charge your customers this amount, it is ultimately up to you as to the price you set for the product, bearing in mind that  Viator will then invoice you for the **merchant net rate** (`merchantNetPriceFrom`) of $555.37 <u>plus</u> a **transaction fee** calculated as a percentage of the net rate; i.e., the **merchant total price**. 

**The exact value of this transaction fee is detailed in your contract with Viator.**