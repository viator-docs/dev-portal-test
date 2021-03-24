# Content ingestion and caching strategy

Much of the information you will need to retrieve from the Viator API – such as the taxonomy, product lists and product details – do not change frequently.

Therefore, we recommend implementing a caching strategy in order to eliminate unnecessary traffic to Viator’s servers and improve the operation of your site. 

This section discusses the different strategies for retrieving and caching Viator’s product catalogue.

You will need to decide on how you will retrieve and manage content from Viator’s product catalogue. The two main options are as follows:

## 1. API response caching

*Partners retrieve content as-needed and cache responses on a service-by-service basis*

If you do not need to store product details locally, we recommend performing caching of on a service-by-service basis; i.e., storing the entire response and applying a time-to-live (TTL) of less than 24 hours.

### Benefits of API response caching

* All the benefits of caching with minimal overhead
* Minimal risk of serving stale or invalid data cached on the partner's side
* No need to download data about products that are not selling
* A smaller volume of local data improves cache hit performance
* Fewer requests made of Viator's systems
* Avoids rate limitations
* Closer adherence to best practices
* Removes need to manage a complex data structure locally

### Service endpoints to cache

Caching should only be applied to services that yield infrequently changing data; i.e.:

* [/taxonomy/destinations](../../../../openapi/reference/operation/taxonomyDestinations)
* [/taxonomy/categories](../../../../openapi/reference/operation/taxonomyCategories)
* [/taxonomy/attractions](../../../../openapi/reference/operation/taxonomyAttractions)
* [/search/products](../../../../openapi/reference/operation/searchProducts)
* [/search/products/codes](../../../../openapi/reference/operation/searchProductsCodes)
* [/search/freetext](../../../../openapi/reference/operation/searchFreetext)
* [/product](../../../../openapi/reference/operation/product)
* [/product/reviews](../../../../openapi/reference/operation/productReviews)
* [/product/photos](../../../../openapi/reference/operation/productPhotos)
* [/available/products](../../../../openapi/reference/operation/availableProducts)

**Note**: these services should be considered cacheable even though some are POST and none include a Cache-Control HTTP header in their response.

## 2. Periodic content ingestion

*Partners download either the full product catalogue or a subset of the catalogue at regular intervals based on destination, linked attraction, or product category filters.*

### Who should use periodic ingestion
This approach may be preferable for partners whose requirements include:
* **System agnosticism/data centralization** – i.e., partners who are simultaneously selling products from vendors other than Viator, have existing product databases or are likely to want to maintain a central product catalogue with a unified taxonomy / data structure
* **Enhanced search capability** – i.e., the ability to apply different categorization rules, filters, exclusions or search optimizations to the product catalogue; e.g., grouping or filtering products according to criteria other than those supported directly by the Viator API (destination, attraction-link or category)

### Frequency of content ingestion
We recommend that you perform an ingestion of the product catalogue once every 24 hours.

### How to retrieve product codes

Make a call to one of the product search services:

* [/search/products](../../../../openapi/reference/operation/searchProducts) – to search by `destId` (destination), `catId` (category), `subCatId` (subcategory) or `seoId` (attraction)
* [/search/freetext](../../../../openapi/reference/operation/searchFreetext) – free-text search across all identifying fields 

### How to retrieve all products in the catalogue

To retrieve all products from the Viator catalogue:

* Retrieve all available destination identifiers (`destId`) from the [/taxonomy/destinations](../../../../openapi/reference/operation/taxonomyDestinations) service
* Iterate through the complete list of `destId`s you retrieved in the previous step, and call [/search/products](../../../../openapi/reference/operation/searchProducts) for each `destId`

**Note**: As some products operate in multiple destinations, the same product code may be returned for a range of different destinations. Therefore, make sure your list of product codes only contains one copy of each code.

You may then iterate through this list of product codes to retrieve any other product details necessary in order to properly populate your local database with the information you require.

### Retrieving a subsection of the product catalogue

You may wish to retrieve only some of the products available in the Viator catalogue; for example, if your organization is only interested in selling products that operate locally.

Your top level search using [/search/products](../../../../openapi/reference/operation/searchProducts) is restricted to one of the three main categorization methods for products; i.e., destination, category/subcategory, or attraction-link; however, you may employ your own methods to filter the selection of products based on any attribute in the product data structure.

### Dealing with pagination using `totalCount` and `topX`

Due to the large number of results that can be returned by the [/search/products](../../../../openapi/reference/operation/searchProducts) service, the request might exceed the 30-second time-out limitation on both sandbox and live servers. Therefore, you will need to make multiple requests to this service including pagination information in order to retrieve all products that match your search criteria.

This is accomplished by sequentially requesting successive segments of the results using the `topX` request parameter together with the `totalCount` response field; i.e.:

* For your first request, specify a `topX` of `"1-100"`
  - **Note**: this range is *inclusive*; i.e., `"topX": 1-100"` will yield the first 100 records 
* The first response will indicate the total number of records available through the value of the `totalCount` field in the response object; e.g.: `"totalCount": 13843`
* For each subsequent request, specify the next logical 'chunk' of data via the `topX` parameter of the request; e.g.:
  - "topX": "1-100"
  - "topX": "101-200"
  - ...
  - "topX": "13801-13843"

### Rate limiting

Due to the heavy load that pre-caching can place on Viator's servers and the downstream servers we connect to, we apply a rate limit of **150 requests per 10 second time window**.

Request rates exceeding this limit will result in a **HTTP 429 (Too Many Requests)** status code being returned.

**Note**: The rate is calculated over a rolling 10-second time window

* In order to avoid running-up against rate limits:
  - insert a delay of 2s if you receive a HTTP 429 status code
  - do not run this as a multi-threaded process