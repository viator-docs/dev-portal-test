# Localization and translation

## Foreign language products

The products available through the Viator API have been created in a variety of languages, often by the suppliers of those products themselves. 

Although the majority of these have been created in English, many have been created in other languages. For example, a tour that operates in Paris might have been created in French.

Viator provides translation services to localize product descriptions to the language of the locale in which they are being presented. In this way, products with descriptions – for example, in French – can be displayed in English on English-language websites. Conversely, products with English-language-descriptions can be displayed in French on French-language websites.

 * **Note**: product descriptions are translated into the language specified in the `Accept-Language` header parameter in the request to each endpoint. 

## Human and machine translation

Some products have been translated by actual humans – 'human translated' –  while others have been automatically translated using Google Translate – 'machine translated'.

The type of translation that has been applied to a product (if any) is indicated by its `translationLevel`, a numeric specifer with meanings as follows:

| `translationLevel` | Meaning |
|-----------------|---------|
| `0` | The product was created by the supplier in the language you specifed using the `Accept-Language` header parameter in the request; i.e., the natural-language text in this response has not been translated |
| `80` | All product information has been <u>machine translated</u> |
| `90` or `100` | All product information has been <u>human translated</u> |

Therefore, any product with a non-zero `translationLevel` has been translated either by a human or via an automatic process.

The `translationLevel` field is returned in the response objects from the following services:

* [/search/products](../../../../openapi/reference/operation/searchProducts)
* [/search/products/codes](../../../../openapi/reference/operation/searchProductsCodes)
* [/search/freetext](../../../../openapi/reference/operation/searchFreetext)
* [/product](../../../../openapi/reference/operation/product)
* [/available/products](../../../../openapi/reference/operation/availableProducts)

When performing a product search using any of these services, you will receive - by default - products with a `translationLevel` of:

* `0` (products that are in the language you specified in `Accept-Language` and are configured for your API-keys), and
* `90` or `100` (products that have been <u>fully human translated</u>)

## Accessing machine-translated products

If your implementation can support the large number of products available that are machine translated, you can.

However, access to the considerable volume of machine-translated products (level `80`), is <u>not granted by default</u>.

To access machine-translated products, you will need to: 

1. Request access to machine-translated products in sandbox from your Business Development account manager.
2. Test your site with sandbox to ensure that you can download and display all the available content.
3. Have your Business Development account manager review your implementation and grant access in the production environment.
