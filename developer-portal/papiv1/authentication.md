# Authentication

**Note**:

- This API now supports two API-key-based authentication methods. Partners can authenticate using the original query-based API-key, or a new style of API-key that is included in each call as a <u>header parameter</u>.

- Partners who are currently authenticating via the [API key query parameter](#legacy-api-key) can continue to authenticate in this way; however, access to the new booking cancellation endpoints ([/bookings/cancel-reasons](../../../openapi/reference/operation/cancellationReasons), [/bookings/cancel-quote](../../../openapi/reference/operation/cancelBookingQuote) and [/bookings/cancel](../../../openapi/reference/operation/cancelBooking)) does require the new-style of API key. All other endpoints remain compatible with both authentication methods.

- If you would like to switch to the [new style of key](#api-key), please speak to your business development account manager.

## API key

Access to the API is managed using an **API key** that is included as a **header parameter** to every call made to all API endpoints described in this document.

| Header parameter name | Example value |
|-----------------------|---------------|
| exp-api-key | bcac8986-4c33-4fa0-ad3f-75409487026c |

If you do not know the API key for your organization, please contact your business development account manager for these details.

Please note that language localization is now controlled on a per-call basis. Previously, language localization was controlled via API-key configuration, with one language available per API key. Under the present scheme, you can access any language enabled for your organization's point of sale via a **single API key**. 

Language selection is accomplished by specifying the desired language as a header parameter (`Accept-Language`). See [Accept-Language header](../appendices/#accept-language-header) for available language codes. If you would like access to additional languages, please contact your business development account manager.

## Legacy API key

Previously, authenticating to this API was accomplished by passing an API-key **as a query parameter** appended to the URI for each call; e.g.:

```html
GET https://viatorapi.sandbox.viator.com/service/taxonomy/destinations?apiKey=xxxxxxxxxxxxxxxxxx
```

This method of authentication remains available for backwards-compatibility with existing implementations. If you would like to upgrade to the new style of API key, please contact your business development account manager. 