# Special offers and on-sale pricing

Suppliers have the option of setting special pricing deals for their products. When a product is 'on sale'; i.e., has a temporarily lowered price, it will be reflected in the product content response, as follows:

| Field name | Standard pricing | Special offer / on-sale pricing |
|------------|----------------|-------------------------|
| `specialOfferAvailable` | `false` | `true` |
| `specialOffer` | `""` (empty string) | e.g.: `"Book by February 28 to save 10%"` |
| `rrp` | `0.0` | pre-discount price |
| `rrpFormatted` | `""` (empty string) | currency-formatted pre-discount price |
| `price` | standard price | special offer price |
| `priceFormatted` | currency-formatted **standard price** | currency-formatted **special-offer price** |
| `merchantNetPriceFrom` | standard merchant net rate | special offer merchant net rate |
| `merchantNetPriceFromFormatted` | currency formatted standard merchant net rate | currency formatted special offer merchant net rate |
| `priceFormatted` | currency-formatted **standard price** | currency-formatted **special-offer price** |
| `priceFrom` (in `tourgrades`) | standard price | special offer price |
| `priceFromFormatted` (in `tourgrades`) | currency-formatted **standard price** | currency-formatted **special-offer price** |

You can use this information to highlight which products are on special and provide details to the user about the special offer.