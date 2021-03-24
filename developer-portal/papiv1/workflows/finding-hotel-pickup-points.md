# Finding hotel pick-up points

## Hotel pickup example:

**Example response body ([/booking/hotels](../../../../openapi/reference/operation/bookingHotels))**:
```javascript
{
  "vmid":"221002",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2012-04-12T13:48:27+0000",
  "success": true,
  "errorReference": null,
  "errorMessageText": null,
  "totalCount": 1,
  "errorName": null
  "data": [
    {
      "address": null,
      "name": "I live locally / I'm staying with friends, relatives",
      "id": "local",
      "phone": null,
      "productCodes": null,
      "destinationId": 0,
      "city": null,
      "notes": null,
      "latitude": null,
      "longitude": null,
      "postcode": null,
      "brand": null,
      "hotelString": "I live locally / I'm staying with friends, relatives",
      "sortOrder": 1
    },
    {
      "address": null,
      "name": "My hotel is not yet booked",
      "id": "notBooked",
      "phone": null,
      "productCodes": null,
      "destinationId": 0,
      "city": null,
      "notes": null,
      "latitude": null,
      "longitude": null,
      "postcode": null,
      "brand": null,
      "hotelString": "My hotel is not yet booked",
      "sortOrder": 2
    },
    {
      "address": null,
      "name": "My hotel is not listed",
      "id": "notListed",
      "phone": null,
      "productCodes": null,
      "destinationId": 0,
      "city": null,
      "notes": null,
      "latitude": null,
      "longitude": null,
      "postcode": null,
      "brand": null,
      "hotelString": "My hotel is not listed",
      "sortOrder": 3
    },
    {
      "address": "375 East Harmon Avenue",
      "name": "Alexis Park Resort Hotel",
      "id": "684_2",
      "phone": "",
      "productCodes": null,
      "destinationId": 684,
      "city": "Las Vegas",
      "notes": null,
      "latitude": 36.106258,
      "longitude": -115.156146,
      "postcode": "89169",
      "brand": "",
      "hotelString": null,
      "sortOrder": 4
    },
    {
      "address": "167 East Tropicana Avenue",
      "name": "Americas Best Value Inn",
      "id": "684_3",
      "phone": "",
      "productCodes": null,
      "destinationId": 684,
      "city": "Las Vegas",
      "notes": null,
      "latitude": 36.100778,
      "longitude": -115.165522,
      "postcode": "89109",
      "brand": "",
      "hotelString": null,
      "sortOrder": 5
    },
    {
      "address": "3131 Las Vegas Boulevard South",
      "name": "Wynn Resort",
      "id": "684_126",
      "phone": "",
      "productCodes": null,
      "destinationId": 684,
      "city": "Las Vegas",
      "notes": null,
      "latitude": 36.127563,
      "longitude": -115.167704,
      "postcode": "89109",
      "brand": "",
      "hotelString": null,
      "sortOrder": 119
    }
  ]
}
```