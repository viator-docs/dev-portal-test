# Overview

The Viator Partner API comprises a set of endpoints that can support the operation of a fully-featured tours and experiences booking website or application; or, it can be integrated with your existing travel-booking software.

The API exposes a variety of services that allow the retrieval of all product details, such as descriptions, pricing, terms and conditions, photos and reviews. This data can either be ingested periodically and managed on your local system, or calls can be made in real time to retrieve content in response to your users' activity on your systems.

The API allows product availability schedules to be retrieved in bulk or queried in real-time, and it can perform pricing calculations according to the number and type of traveler for the wide variety of product option combinations typically available in the tours and activities sales workflow.

The API provides booking and post-booking functionality, allowing booking requests, ticket purchase, and booking status updates.

Various utility services are available to map between yours and Viator's data taxonomy.

**Please note:** The API does not provide services for storing data, such as user accounts. We assume that merchant partners have their own systems for storing this data.

## Who is the API for?

The Viator Partner API is designed for use by organizations and individuals partnered with Viator in one of the following capacities:

### Merchant partners

A merchant partner is one who operates as the merchant of record; i.e., takes full responsibility for all monetary transactions carried out by their users, as well as providing customer support with regard to providing help, processing cancellations and refunds, and liaising between suppliers and customers when the need to communicate information arises.

Merchant partners are invoiced periodically by Viator for all product sales. You will need to demonstrate that you have access to the appropriate infrastructure to effectively support the requisite business operations in order to become a merchant partner.

### Viator Branded Affiliates (VBAs)

VBAs have full access to the areas of the API relating to content, but sales of Viator products must be carried out on the Viator site itself; therefore, access to the booking or transactional endpoints necessary to operate as the merchant of record (i.e., merchant partners) is restricted. 

When a customer wishes to book a product from a VBA partner's site, they are instead redirected to [viator.com](https://viator.com) in order to complete the purchase; whereas, merchant partners are able to process and manage bookings through the Viator API itself, allowing their customers to book products without leaving the partner's site.

Viator affiliates instead generate unique URLs that redirect their users to the Viator site, resulting in a cookie being set such that all transactions will accrue a commission for that partner until the cookie expires.

Purchases of products originating from the VBAs site are recorded and a commission on these sales is paid periodically.

- **Note**: VBA partners should refer to a different document for technical specifications relevant to their partner type. If you are a VBA partner, please navigate to: [https://docs.viator.com/partner-api/affiliate/technical/](https://docs.viator.com/partner-api/affiliate/technical)

### VBAs with booking capability

VBAs also have the option of allowing their customers to process bookings directly from their site and via the API – similar to a merchant partner – without being redirected to viator.com to complete their transaction. This partner type sends customer details, product details and credit card payment information via the API, but Viator retains control of and responsibility for processing payments and customer support.

### White label partners

White label partners do not operate their own site infrastructure. Instead, Viator provides a white label site with full functionality that can be branded according to the partner’s wishes.

## Uses of the Viator Partner API

The Viator Partner API is used to carry out the following tasks:

### Product search and ingestion

Partners can use the product search endpoints to retrieve lists of products from Viator’s inventory relevant to their business. The available search criteria include:

- The location (destination) in which the product operates
- Whether the product is associated with a well-known tourist attraction; e.g., Empire State Building
- The type of product (known as its category and/or subcategory)
- The time period during which the product operates 
- Words or phrases that occur in a product's description via a free-text search

Partners who prefer to download product details periodically (instead of performing all operations in real time in response to user behavior) do so by using the product search endpoints to compile a list of products that they wish to sell on their site. They then download comprehensive product details for each via the /product endpoint.

#### Product search endpoints:

| Endpoint | Use |
|-|-|
| [/search/products](../../../openapi/reference/operation/searchProducts) | Allows searching for products according to: destination / location, relationship to a known tourist attraction; category and/or subcategory; date of operation |
| [/search/products/codes](../../../openapi/reference/operation/searchProductsCodes) | Retrieves product details for products that match a list of product codes (unique identifiers for the product) |
| [/search/freetext](../../../openapi/reference/operation/searchFreetext) | Retrieves product details for products that include the search terms in the product's description and details. |
| [/available/products](../../../openapi/reference/operation/availableProducts) | Retrieves products that are identified by specific product codes, operate during a specified day range and accept a certain number of adult travelers |

#### Product information endpoints:

All information about a product that must be communicated to customers prior to purchase is available via [/product](#operation/product) and its auxiliary endpoints. This content is generally used to construct product display pages and for performing local searches.

Important information about a product includes:

- Product and supplier names
- Geographic location
- Product description
- Category and subcategory
- Photos (from both users and the supplier)
- User reviews and ratings
- Product options (variants of the tour/activity, such as starting times, passenger mix options, and inclusions/add-ons, including basic pricing information for each)
- Which age ranges can participate
- Booking details
- Cancellation policies
- Basic pricing
- Logistics
  + Inclusions (e.g., provided meals)
  + Exclusions (e.g., entrance fees to visited attractions)
  + Health restrictions and accessibility
  + Departure times
  + Passenger pick-up
  + Duration
  + Tour routes

### Availability

The availability of a tour is communicated via the API's availability endpoints. The availability aspects of a product include:

- On which days and at which times the product is available to be booked
- Whether the product option (variant) supports a certain combination of passengers according to age and number
- Pricing information

Some partners choose to bulk-ingest availability information for their products in order to expedite this part of the customer workflow or to facilitate a local search functionality on their website; but, because this information changes very regularly, a final real-time call is generally recommended to ensure that when the booking request is submitted by the customer it is unlikely to fail on account of a change in availability.

#### Availability endpoints

| Endpoint | Use |
|-|-|
| [/booking/availability](../../../openapi/reference/operation/bookingAvailability) | Returns the product option with the lowest price that is available on each day |
| [/booking/availability/dates](../../../openapi/reference/operation/bookingAvailabilityDates) | Returns all available dates for a product (but without regard to product option) |
| [/booking/availability/tourgrades](../../../openapi/reference/operation/bookingAvailabilityTourgrades) | Returns all product options for a product that are available on the specified day for the specified passenger mix |
| [/booking/availability/tourgrades/pricingmatrix](../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) | Returns a detailed matrix of product options, passenger mixes and the pricing applicable to each combination |
| [/booking/calculateprice](../../../openapi/reference/operation/bookingCalculateprice) | Provides a reconfirmation of a product's availability with respect to the product option and passenger mix provided and calculates a final price; used as a final availability check immediately prior to making a booking |

### Booking and cancellations

Merchant partners and VBAs with booking capabilities can use the Viator Partner API to purchase the product through the [/booking/book](../../../openapi/reference/operation/bookingBook) endpoint.

The API also provides services to:

- Enquire about the status of an existing booking
- Retrieve tickets/vouchers for the product
- Cancel a booking

#### Booking endpoints

| Endpoint | Use |
|-|-|
| [/booking/book](../../../openapi/reference/operation/bookingBook) | Make a booking / purchase a product |
| [/booking/status](../../../openapi/reference/operation/bookingStatus) | Retrieve multiple detailed booking statuses based on a range of specified criteria |
| [/booking/status/items](../../../openapi/reference/operation/bookingStatusItems) | Similar to [/booking/status](../../../openapi/reference/operation/bookingStatus), but provides slightly less detail and can be called more frequently |

#### Cancellation endpoints

| Endpoint | Use |
|-|-|
| [/bookings/{booking-reference}/cancel-quote](../../../openapi/reference/operation/bookingQuote) | Returns the expected outcome of a booking cancellation request (taking into consideration the product's cancellation policy) were the cancellation request performed immediately |
| [/bookings/{booking-reference}/cancel](../../../openapi/reference/operation/cancelBooking) | Cancels the booking and assigns a refund depending on the product’s cancellation policy |

### Auxiliary services

Taxonomical data sets are required to interact meaningfully with the Viator Partner API; for example, mappings from destination (location of operation) to their respective identification codes. This information may occasionally change or be added to. Consequently, the API includes endpoints that return the most up-to-date versions of this information.

#### Taxonomy endpoints

| Endpoint | Use |
|-|-|
| [/taxonomy/destinations](../../../openapi/reference/operation/taxonomyDestinations) | Retrieves a list of destination names, types and unique identifiers to be used when interacting with the Viator Partner API |
| [/taxonomy/categories](../../../openapi/reference/operation/taxonomyCategories) | Retrieves a list of product categories for a destination that can be used as a means of filtering when searching for products using the [/search/products](../../../openapi/reference/operation/searchProducts) endpoint |
| [/taxonomy/attractions](../../../openapi/reference/operation/taxonomyAttractions) | Retrieves a list of tourist attractions (e.g., the Eiffel Tower or Empire State Building) and their associated identification codes to be used as a means of searching for available products; for example, in the [/search/products](../../../openapi/reference/operation/searchProducts) service |
| [/booking/hotels](../../../openapi/reference/operation/bookingHotels) | Retrieves a list of hotels, including names and geographic locations, to be used when making booking requests |

---