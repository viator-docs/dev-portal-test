# Get the booking status for multiple items

The [/booking/status](../../../../openapi/reference/operation/bookingStatus) service retrieves the booking status for multiple items based on different criteria.

This service can only be polled every 30 minutes. This would ideally be used in software for bulk updates of pending itineraries.

The maximum number of results returned is 1,000 itineraries. Narrow your search if you expect results greater than this.

**NOTE:** This will return both live and test bookings.

**Example request body**

```javascript
{
  "bookingDateFrom": "2013-03-22",
  "bookingDateTo": "2013-03-25",
  "itineraryIds": null,
  "itemIds": null,
  "distributorRefs": ["Ref20132603_1","Ref20132603_5"],
  "distributorItemRefs": null,
  "leadFirstName": null,
  "leadSurname": null,
  "test": true
}
```

All fields are optional and can be omitted, however at least one needs to be provided.

| Field | Meaning |
|-------|---------|
| `bookingDateFrom` | The booking date is greater than or equal this date |
| `bookingDateTo` | The booking date is less than or equal this date |
| `itemIds` | Array of item ids (AKA Viator Item Reference) to check for; e.g., `[1234657,2345267,3245154]` |
| `distributorRefs` | Array of partner-defined distributor references; e.g., `["ref1","ref2","ref3"]` |
| `distributorItemRefs` | Array of partner-defined distributor item references; e.g., `["refItem1","refItem2","refItem3"]` |
| `leadFirstName` | The lead traveller's first name |
| `leadSurname` | The lead traveller's surname |
| `test` | Setting `test` to `true` will bypass the poll limit on the sandbox environment only. The default value for `test` is `false`. |

**Example response object** ([/booking/status](../../../../openapi/reference/operation/bookingStatus))

```javascript
{
  "data": [
  {
    "itineraryId": 3332064,
    "bookingStatus": {
      "type": "CONFIRMED",
      "level": "ITINERARY",
      "failed": false,
      "text": "Confirmed",
      "cancelled": false,
      "status": 3,
      "confirmed": true,
      "amended": false,
      "pending": false
    },
    "bookingDate": "2013-03-25",
    "distributorRef": "Ref20132603_1",
    "itemSummaries": [{
      "itineraryId": 3332064,
      "itemId": 600088886,
      "bookingStatus": {
        "type": "CONFIRMED",
        "level": "ITEM",
        "failed": false,
        "text": "Paid &amp; Confirmed",
        "cancelled": false,
        "status": 1,
        "confirmed": true,
        "amended": false,
        "pending": false
      },
      "travelDate": "2013-12-03",
      "distributorItemRef": "ItemRefA",
      "sortOrder": 0
    }],
    "sortOrder": 1
  },
  {
    "itineraryId": 3332076,
    "bookingStatus": {
      "type": "CONFIRMED",
      "level": "ITINERARY",
      "failed": false,
      "text": "Confirmed",
      "cancelled": false,
      "status": 3,
      "confirmed": true,
      "amended": false,
      "pending": false
    },
    "bookingDate": "2013-03-26",
    "distributorRef": "Ref20132603_5",
    "itemSummaries": [{
      "itineraryId": 3332076,
      "itemId": 600088907,
      "bookingStatus": {
        "type": "CONFIRMED",
        "level": "ITEM",
        "failed": false,
        "text": "Paid &amp; Confirmed",
        "cancelled": false,
        "status": 1,
        "confirmed": true,
        "amended": false,
        "pending": false
      },
      "travelDate": "2013-12-03",
      "distributorItemRef": "ItemRefA",
      "sortOrder": 0
    }],
    "sortOrder": 2
  }],
  "vmid": "221002",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2013-03-26T10:25:57+0000",
  "errorReference": null,
  "errorMessageText": null,
  "success": true,
  "totalCount": 2,
  "errorName": null
}
```

## Exceeding the poll limit

You will receive the following error if you exceed the number of calls allowed to the service within the timeframe:

```javascript
{
  "data": null,
  "vmid": "221002",
  "errorMessage": [
    "Access allowed every 30 minutes"
  ],
  "errorType": "EXCEPTION",
  "dateStamp": "2013-03-26T10:28:51+0000",
  "errorReference": "~55315512721712161381352771",
  "errorMessageText": [
    "Access allowed every 30 minutes"
  ],
  "success": false,
  "totalCount": 1,
  "errorName": "PollingDeniedException"
}
```