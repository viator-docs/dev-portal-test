# Understanding the pricingUnit field

This section explains the meaning and function of the `pricingUnit` field in the response object received from the [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) service.

## Making the request
The [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) service takes the following parameters as input in its request body:

| Parameter | Type | Meaning |
|-----------|------|---------|
| `productCode` | string | unique alphanumeric identifier of the product to enquire about; e.g., `10040WORLD` |
| `month` | string | **month** of the year by which to filter results |
| `year` | string | **year** by which to filter results |
| `currencyCode` | string | **currency code** for the currency in which to display pricing information |

### Example request body

Sending the following request body to the [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) service will retrieve pricing information for "Skip the Line: World of Discoveries Entrance Ticket in Porto" (product code: 10040WORLD).

```javascript
{
  "productCode": "10040WORLD",
  "currencyCode": "USD",
  "month": "06",
  "year": "2019"
}
```

## Interpreting the response

Within each object item of the `tourGrades` array – each of which gives the pricing details for a specific tour grade – is a `pricingMatrix` array. Each object in this array details the per-age-band pricing for a specific pricing unit (`pricingUnit`) associated with that product.

Using this information, you should be able to calculate the total cost of the booking – considering any booking options – with regard to the number of participants.

How to identify the fundamental unit prices for a booking, which are then summed in order to calculate a total price, is detailed below.

Note that in the response object received from the service, pricing schedules are organized hierarchically as follows:

```c
bookingDate
--tourGrade
----ageBand
------price
```

## Types of pricing unit
There are two fundamental types of pricing unit – **per-person** and **per-group**.

### Per-person pricing schedule
If the pricing is *per-person*, then the total price of the booking will be directly proportional to the number of participants (passengers) of each type that are booking the product; i.e., a direct multiple of the per-person price.

The only pricing unit specifier for *per-person* pricing is “per person”, given in the `pricingUnit` field; i.e.:

- `"pricingUnit": "per person"`

| Pricing unit | Example product | Meaning |
|--------------|---------|---------|
| `"per person"` | **10040WORLD** | **Per-person pricing** – the unit price refers to the price for an individual participant.<br />Some products have tiered pricing arrangements; i.e., a different per-person price can apply if certain numbers and combinations of participants in a particular age band are booking the product; e.g.:<br /><ul><li>**1-2 adults**: $50 per person</li><li>**3-4 adults**: $45 per person</li></ul><br />Whether a range is available to be booked depends on whether the customer’s desired passenger mix satisfies the `minimumCountRequired` and `maximumCountRequired` fields in each item of the `ageBandPrices` array. |

### Per-group pricing
If the pricing is per-group, then the total price of the booking will depend on the number of groups and types of group that ideally accommodate the participant mix.

The following pricing schedules follow “per-group” logic:

- `"pricingUnit": "per vehicle"`
- `"pricingUnit": "per car"`
- `"pricingUnit": "per group"`
- `"pricingUnit": "per boat"`
- `"pricingUnit": "per package"`
- `"pricingUnit": "per jetski"`
- `"pricingUnit": "per vessel"`
- `"pricingUnit": "per helicopter"`
- `"pricingUnit": "per room"`
- `"pricingUnit": "per bike"`
- `"pricingUnit": "per flight"`
- `"pricingUnit": "per plane"`
- `"pricingUnit": "per couple"`

Eligibility for a certain individual pricing schedule; or, for inclusion in a particular group type, depends on the tour grade for the product, the type of participant (e.g., the age-band they fall into) and the date of the booking.

### Group pricing schedules

| Pricing unit  | Example product | Meaning |
|---------------|-----------|---------|
| `"per group"` | **10847P42**  | **Per-group** pricing – the unit price is calculated according to the number of groups the specified passenger will fit into rather than the exact number of participants. `minimumCountRequired` and `maximumCountRequired` must be considered as these fields relate to the available group sizes. |
| `"per room"`  | **6279P26** | **Per-room** pricing relates the room price, which depends on the number of participants making the booking. |
| `"per package"` | **25941P70**  | **Per-package** pricing refers to products that are sold as part of a package; for example a family package stipulating a passenger mix of two adults and two children |
| `"per vehicle"` | **6154SHOP**  | **Per-vehicle** pricing is calculated according to the number of vehicles required for the specified passenger mix rather than the exact number of participants. `minimumCountRequired` and `maximumCountRequired` must be considered as these fields relate to the occupancy limitations for each vehicle. The minimum price will depend on the rate for a single vehicle. |
| `"per car"` | **10175P10**  | **Per-car** pricing – identical to "per vehicle", but refers specifically to vehicles that are cars. |
| `"per boat"`  | **11121P40**  | **Per-boat** pricing – identical to "per vehicle", but refers specifically to vehicles that are boats. |
| `"per jetski"`  | **28965P127** | **Per-jetski** pricing – identical to "per vehicle", but refers specifically to vehicles that are jet-skis. |
| `"per vessel"`  | **17295P24**  | **Per-vessel** pricing – identical to "per vehicle", but refers specifically to maritime vessels that are not strictly boats. |
| `"per helicopter"`  | **12189P23**  | **Per-helicopter** pricing – identical to "per vehicle", but refers specifically to vehicles that are helicopters. |
| `"per bike"`  | **17448P8** | **Per-bike** pricing – identical to "per vehicle", but refers specifically to vehicles that are bikes. |
| `"per flight"`  | **28965P134** | **Per-flight** pricing – identical to "per vehicle", but refers specifically to the act of being aboard a flying vehicle. |
| `"per plane"` | **14876P5** | **Per-plane** pricing – identical to "per vehicle", but refers specifically to vehicles that are aeroplanes. |

## Interpreting response objects by example

In this section, we’ll have a look at snippets from the response objects received from the [/booking/availability/tourgrades/pricingmatrix](../../../../openapi/reference/operation/bookingAvailabilityTourgradesPricingmatrix) service and interpret the results.

### Per-person pricing
**Request object**:
```javascript
{
  "productCode": "10040WORLD",
  "currencyCode": "USD",
  "month": "06",
  "year": "2019"
}
```
**Response snippet**:

**Note:** `pricingMatrix` is an array of objects that detail the available pricing schedules for the product:

```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per person",
    "bookingDate": "2019-06-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 13.85,
            "priceFormatted": "$13.85",
            "merchantNetPrice": 11.05,
            "merchantNetPriceFormatted": "$11.05",
            "minNoOfTravellersRequiredForPrice": 1
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 0,
        "maximumCountRequired": 15
      },
      {
        "bandId": 2,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 6.92,
            "priceFormatted": "$6.92",
            "merchantNetPrice": 5.53,
            "merchantNetPriceFormatted": "$5.53",
            "minNoOfTravellersRequiredForPrice": 1
          }
        ],
        "sortOrder": 2,
        "minimumCountRequired": 0,
        "maximumCountRequired": 15
      },
      {
        "bandId": 3,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 0,
            "priceFormatted": "$0.00",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00",
            "minNoOfTravellersRequiredForPrice": 1
          }
        ],
        "sortOrder": 3,
        "minimumCountRequired": 0,
        "maximumCountRequired": 15
      },
      {
        "bandId": 5,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 10.39,
            "priceFormatted": "$10.39",
            "merchantNetPrice": 8.3,
            "merchantNetPriceFormatted": "$8.30",
            "minNoOfTravellersRequiredForPrice": 1
          }
        ],
        "sortOrder": 4,
        "minimumCountRequired": 0,
        "maximumCountRequired": 15
      }
    ]
  }
]
```

In this example, four age bands (1, 2, 3 and 5) have pricing information available. These numerically-identified age bands are the age bands allowed to book the product. Details of the age ranges that the product operator has defined are available from the **/product** service.

A call to **/product** regarding `10040WORLD` yields the following information:

```javascript
"ageBands": [
  {
    "sortOrder": 1,
    "ageFrom": 13,
    "ageTo": 64,
    "adult": true,
    "bandId": 1,
    "pluralDescription": "Adults",
    "treatAsAdult": true,
    "count": 0,
    "description": "Adult"
  },
  {
    "sortOrder": 2,
    "ageFrom": 65,
    "ageTo": 99,
    "adult": false,
    "bandId": 5,
    "pluralDescription": "Seniors",
    "treatAsAdult": true,
    "count": 0,
    "description": "Senior"
  },
  {
    "sortOrder": 3,
    "ageFrom": 4,
    "ageTo": 12,
    "adult": false,
    "bandId": 2,
    "pluralDescription": "Children",
    "treatAsAdult": false,
    "count": 0,
    "description": "Child"
  },
  {
    "sortOrder": 4,
    "ageFrom": 0,
    "ageTo": 3,
    "adult": false,
    "bandId": 3,
    "pluralDescription": "Infants",
    "treatAsAdult": false,
    "count": 0,
    "description": "Infant"
  }
```

Product operators choose the age bands available for their product from the following five categories and define the age ranges that pertain to each band; i.e.:

| `bandId` | `description` |
|:----------:|:-----------:|
| **1** | Adult |
| **2** | Child |
| **3** | Infant |
| **4** | Youth |
| **5** | Senior |

For this product, the age bands have been defined as follows:

| `bandId` | `description` | `ageFrom` | `ageTo` |
|:--------:|:-----------:|:-------:|:-----:|
| **1** | Adult | 13 | 64 |
| **5** | Senior | 65 | 99 |
| **2** | Child | 4 | 12 |
| **3** | Infant | 0 | 3 |

Therefore, for this product, the following pricing applies:

| Passenger type | Number | Price |
|----------------|:------:|-------|
| **Adult** | 1-15 | $13.85 per person |
| **Senior** | 1-15 | $10.39 per person |
| **Child** | 1-15 | $6.92 per person |
| **Infant** | 1-15 | free ($0) |

Per-person pricing might depend on the mix of passengers booking the tour. In the following example (`5010SYDNEY`), a "48 Hour Family Pass Ticket" has a different price for children depending on how many are participating, which we'll see in the following snippet.

**Response snippet**:
```javascript
"tourGrades": [
  {
    "sortOrder": 1,
    "gradeCode": "14HFAM",
    "gradeTitle": "48 Hour Family Pass Ticket",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 133.47,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$133.47",
                "merchantNetPrice": 106.62,
                "merchantNetPriceFormatted": "$106.62"
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 1
          },
          {
            "bandId": 2,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 0,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$0.00",
                "merchantNetPrice": 0,
                "merchantNetPriceFormatted": "$0.00"
              }
            ],
            "sortOrder": 2,
            "minimumCountRequired": 2,
            "maximumCountRequired": 2
          },
          {
            "bandId": 3,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 0,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$0.00",
                "merchantNetPrice": 0,
                "merchantNetPriceFormatted": "$0.00"
              }
            ],
            "sortOrder": 3,
            "minimumCountRequired": 0,
            "maximumCountRequired": null
          }
        ]
      },
      {
        "sortOrder": 2,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 133.47,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$133.47",
                "merchantNetPrice": 106.62,
                "merchantNetPriceFormatted": "$106.62"
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 1
          },
          {
            "bandId": 2,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 3.71,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$3.71",
                "merchantNetPrice": 2.96,
                "merchantNetPriceFormatted": "$2.96"
              }
            ],
            "sortOrder": 2,
            "minimumCountRequired": 3,
            "maximumCountRequired": 4
          },
          {
            "bandId": 3,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 0,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$0.00",
                "merchantNetPrice": 0,
                "merchantNetPriceFormatted": "$0.00"
              }
            ],
            "sortOrder": 3,
            "minimumCountRequired": 0,
            "maximumCountRequired": null
          }
        ]
      }
    ]
  },
  …
]
```

**Interpretation**:

To be eligible for a family pass ticket, the group must consist of an adult and at least two children.

| Passenger mix | Adult price | Child price | Infant price |
|---------------|:-----------:|:-----------:|:------------:|
| 1 Adult +<br />1 Child | N/A | N/A | N/A |
| 1 Adult +<br />2 Children +<br />Any infants | $133.47 | FREE | FREE |
| 1 Adult +<br />3-4 Children +<br />Any infants | $133.47 | $3.71 | FREE |

### Tiered per-person pricing

In this example, we see a per-person pricing schedule with a tiered arrangement, where the per-person price decreases depending on how many people are booking the tour, but the total price is still calculated as the sum of the individual per-person prices rather than an overall 'group' price.

**Request object**:
```javascript
{
  "productCode": "17972P102",
  "currencyCode": "USD",
  "month": "08",
  "year": "2019"
}
```
**Response snippet**:

```javascript
"tourGrades": [
  {
    "sortOrder": 1,
    "gradeCode": "TG1",
    "gradeTitle": "Arrival transfer",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 52.45,
                "priceFormatted": "$52.45",
                "merchantNetPrice": 40.87,
                "merchantNetPriceFormatted": "$40.87",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "maximumCountRequired": 1,
            "minimumCountRequired": 1
          }
        ]
      },
      {
        "sortOrder": 2,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 26.22,
                "priceFormatted": "$26.22",
                "merchantNetPrice": 20.44,
                "merchantNetPriceFormatted": "$20.44",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "maximumCountRequired": 2,
            "minimumCountRequired": 2
          }
        ]
      },
      {
        "sortOrder": 3,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 17.91,
                "priceFormatted": "$17.91",
                "merchantNetPrice": 13.62,
                "merchantNetPriceFormatted": "$13.62",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "maximumCountRequired": 3,
            "minimumCountRequired": 3
          }
        ]
      },
      {
        "sortOrder": 4,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 19.19,
                "priceFormatted": "$19.19",
                "merchantNetPrice": 14.99,
                "merchantNetPriceFormatted": "$14.99",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "maximumCountRequired": 4,
            "minimumCountRequired": 4
          }
        ]
      },
      {
        "sortOrder": 5,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 15.35,
                "priceFormatted": "$15.35",
                "merchantNetPrice": 12.25,
                "merchantNetPriceFormatted": "$12.25",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "maximumCountRequired": 5,
            "minimumCountRequired": 5
          }
        ]
      },
      {
        "sortOrder": 6,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 12.66,
                "priceFormatted": "$12.66",
                "merchantNetPrice": 10.08,
                "merchantNetPriceFormatted": "$10.08",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "maximumCountRequired": 6,
            "minimumCountRequired": 6
          }
        ]
      },
      {
        "sortOrder": 7,
        "pricingUnit": "per person",
        "bookingDate": "2019-08-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 10.94,
                "priceFormatted": "$10.94",
                "merchantNetPrice": 8.72,
                "merchantNetPriceFormatted": "$8.72",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "maximumCountRequired": 7,
            "minimumCountRequired": 7
          }
        ]
      }
    ]
  }
```

**Interpretation**:

| Travelers | Per-person price | Total price |
|:----------------:|:----------------:|:-----------:|
| 1 | $52.45 | $52.45 |
| 2 | $26.22 | $52.44 |
| 3 | $17.91 | $53.73 |
| 4 | $19.19 | $76.76 |
| 5 | $15.35 | $76.75 |
| 6 | $12.66 | $75.96 |
| 7 | $10.94 | $76.58 |

### Per-group pricing
**Request object**:
```javascript
{
  "productCode": "10847P42",
  "currencyCode": "USD",
  "month": "06",
  "year": "2019"
}
```

**Response snippet**:

```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per group",
    "bookingDate": "2019-06-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 390,
            "minNoOfTravellersRequiredForPrice": 1,
            "priceFormatted": "$390.00",
            "merchantNetPrice": 339.74,
            "merchantNetPriceFormatted": "$339.74"
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 390,
            "minNoOfTravellersRequiredForPrice": 2,
            "priceFormatted": "$390.00",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00"
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 10
      }
    ]
  }
]
```

**Interpretation**:

- "$390 per group of up to 10 adults"

### Per-room pricing

**Request object**:
```javascript
{
  "productCode": "100245P40",
  "currencyCode": "USD",
  "month": "08",
  "year": "2019"
}
```
**Response snippet**:

```javascript
"pricingMatrix": [
{
    "sortOrder": 1,
    "pricingUnit": "per room",
    "bookingDate": "2019-08-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 110,
            "minNoOfTravellersRequiredForPrice": 1,
            "priceFormatted": "$110.00",
            "merchantNetPrice": 95.85,
            "merchantNetPriceFormatted": "$95.85"
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 110,
            "minNoOfTravellersRequiredForPrice": 2,
            "priceFormatted": "$110.00",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00"
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 10
      }
    ]
  }
]
```

**Interpretation**:
- "$110 per group of up to 10 adults"

### Per-package pricing

**Request object**:
```javascript
{
  "productCode": "25941P70",
  "currencyCode": "USD",
  "month": "02",
  "year": "2019"
}
```

**Response snippet**:
```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per package",
    "bookingDate": "2019-02-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 87.7,
            "minNoOfTravellersRequiredForPrice": 1,
            "priceFormatted": "$87.70",
            "merchantNetPrice": 67.23,
            "merchantNetPriceFormatted": "$67.23"
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 87.7,
            "minNoOfTravellersRequiredForPrice": 2,
            "priceFormatted": "$87.70",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00"
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 10
      }
    ]
  }
]
```

**Interpretation**:
- "$87.70 per group of up to 10 adults"

### Per-vehicle pricing

**Request object**:
```javascript
{
  "productCode": "20190P4",
  "currencyCode": "USD",
  "month": "06",
  "year": "2019"
}
```

**Response snippet**:
```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per vehicle",
    "bookingDate": "2019-06-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 250,
            "minNoOfTravellersRequiredForPrice": 1,
            "priceFormatted": "$250.00",
            "merchantNetPrice": 186.38,
            "merchantNetPriceFormatted": "$186.38"
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 250,
            "minNoOfTravellersRequiredForPrice": 2,
            "priceFormatted": "$250.00",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00"
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 7
      }
    ]
  }
]
```

**Interpretation**:
- "$250 per group of up to 7 adults"

### Per-car pricing
**Request object**:
```javascript
{
  "productCode": "10175P10",
  "currencyCode": "USD",
  "month": "06",
  "year": "2019"
}
```

**Response snippet**:

```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per car",
    "bookingDate": "2019-06-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 98.08,
            "priceFormatted": "$98.08",
            "merchantNetPrice": 78.34,
            "merchantNetPriceFormatted": "$78.34",
            "minNoOfTravellersRequiredForPrice": 1
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 98.08,
            "priceFormatted": "$98.08",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00",
            "minNoOfTravellersRequiredForPrice": 2
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 3
      }
    ]
  }
]
```
**Interpretation**:
- "$98.08 per group of up to 3 adults"

### Per-boat pricing
**Request object**:
```javascript
{
  "productCode": "11121P40",
  "currencyCode": "USD",
  "month": "08",
  "year": "2018"
}
```
**Response snippet**:

```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per boat",
    "bookingDate": "2018-06-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 266.21,
            "merchantNetPrice": 226.81,
            "merchantNetPriceFormatted": "$226.81",
            "priceFormatted": "$266.21",
            "minNoOfTravellersRequiredForPrice": 1
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 266.21,
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00",
            "priceFormatted": "$266.21",
            "minNoOfTravellersRequiredForPrice": 2
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 2
      }
    ]
  }
]
```

**Interpretation**:
- "$266.21 per group of up to 2 adults"

### Per-jetski pricing

**Request object**:

```javascript
{
  "productCode": "28965P127",
  "currencyCode": "USD",
  "month": "08",
  "year": "2018"
}
```

**Response snippet**:

```javascript
"tourGrades": [
  {
    "sortOrder": 1,
    "gradeCode": "TG1",
    "gradeTitle": "20 minutes for 1 person",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per jetski",
        "bookingDate": "2018-06-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 55.46,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$55.46",
                "merchantNetPrice": 47.25,
                "merchantNetPriceFormatted": "$47.25"
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 1
          }
        ]
      }
    ]
  },
  {
    "sortOrder": 2,
    "gradeCode": "TG3",
    "gradeTitle": "20 minutes for 2 persons",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per jetski",
        "bookingDate": "2018-06-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 66.55,
                "minNoOfTravellersRequiredForPrice": 1,
                "priceFormatted": "$66.55",
                "merchantNetPrice": 56.7,
                "merchantNetPriceFormatted": "$56.70"
              },
              {
                "sortOrder": 2,
                "currencyCode": "USD",
                "price": 66.55,
                "minNoOfTravellersRequiredForPrice": 2,
                "priceFormatted": "$66.55",
                "merchantNetPrice": 0,
                "merchantNetPriceFormatted": "$0.00"
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 2
          }
        ]
      }
    ]
  }
]
```

**Interpretation**:

This example shows how group prices can differ according to the size of the group in question. In this case, two adults can ride together on a two-person jet ski, whereas a single adult requires his or her own jet ski, and therefore the unit price is slightly higher for the single adult.

| Travelers | Vehicle type | Price per jet ski | Price per person |
|:----------:|--------------|:-----------------:|:----------------:|
| 1 | Single-person jet ski | $55.46 | $55.46 |
| 2 | Two-person jet ski | $66.55 | $33.275 |

### Per-vessel pricing

**Request object**:

```javascript
{
  "productCode": "17295P24",
  "currencyCode": "USD",
  "month": "06",
  "year": "2019"
}
```

**Response snippet**:

```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per vessel",
    "bookingDate": "2019-06-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 799,
            "priceFormatted": "$799.00",
            "merchantNetPrice": 680.75,
            "merchantNetPriceFormatted": "$680.75",
            "minNoOfTravellersRequiredForPrice": 1
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 799,
            "priceFormatted": "$799.00",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00",
            "minNoOfTravellersRequiredForPrice": 2
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 12
      }
    ]
  }
]
```

**Interpretation**:
- "$799.00 per group of up to 12 adults"

### Per-helicopter pricing

**Request object**:

```javascript
{
  "productCode": "12189P23",
  "currencyCode": "USD",
  "month": "08",
  "year": "2018"
}
```

**Response snippet**:
```javascript
"tourGrades": [
  {
    "sortOrder": 1,
    "gradeCode": "TG1",
    "gradeTitle": "Private Helicopter 1 to 2 Pax",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per helicopter",
        "bookingDate": "2018-06-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 1714.83,
                "priceFormatted": "$1,714.83",
                "merchantNetPrice": 1461.03,
                "merchantNetPriceFormatted": "$1,461.03",
                "minNoOfTravellersRequiredForPrice": 1
              },
              {
                "sortOrder": 2,
                "currencyCode": "USD",
                "price": 1714.83,
                "priceFormatted": "$1,714.83",
                "merchantNetPrice": 0,
                "merchantNetPriceFormatted": "$0.00",
                "minNoOfTravellersRequiredForPrice": 2
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 2
          }
        ]
      }
    ]
  },
  {
    "sortOrder": 2,
    "gradeCode": "TG2",
    "gradeTitle": "Private Helicopter 1 to 3 Pax",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per helicopter",
        "bookingDate": "2018-06-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 2047.41,
                "priceFormatted": "$2,047.41",
                "merchantNetPrice": 1744.4,
                "merchantNetPriceFormatted": "$1,744.40",
                "minNoOfTravellersRequiredForPrice": 1
              },
              {
                "sortOrder": 2,
                "currencyCode": "USD",
                "price": 2047.41,
                "priceFormatted": "$2,047.41",
                "merchantNetPrice": 0,
                "merchantNetPriceFormatted": "$0.00",
                "minNoOfTravellersRequiredForPrice": 2
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 3
          }
        ]
      }
    ]
  }
]
```

**Interpretation**:
| Travelers | Vehicle type | Price per helicopter | Price per person |
|:----------:|--------------|:--------------------:|:----------------:|
| 1 | 1-2-person helicopter | $1,714.83 | $1,714.83 |
| 2 | 1-2-person helicopter | $1,714.83 | $857.415 |
| 3 | 1-3-person helicopter | $2,047.41 | $682.47 |

### Per-bike pricing

**Request object**:
```javascript
{
  "productCode": "17448P8",
  "currencyCode": "USD",
  "month": "08",
  "year": "2018"
}
```

**Response snippet**:
```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per bike",
    "bookingDate": "2018-06-01",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 208.53,
            "priceFormatted": "$208.53",
            "merchantNetPrice": 177.67,
            "merchantNetPriceFormatted": "$177.67",
            "minNoOfTravellersRequiredForPrice": 1
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 208.53,
            "priceFormatted": "$208.53",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00",
            "minNoOfTravellersRequiredForPrice": 2
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 2
      }
    ]
  }
]
```

**Interpretation**:

- "$208.53 per bike, with up to two adults per bike"

### Per-flight pricing

**Request object**:

```javascript
{
  "productCode": "28965P134",
  "currencyCode": "USD",
  "month": "08",
  "year": "2018"
}
```

**Response snippet**:

```javascript
"tourGrades": [
  {
    "sortOrder": 1,
    "gradeCode": "TG1",
    "gradeTitle": "Individual flight",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per flight",
        "bookingDate": "2018-06-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 61.01,
                "merchantNetPrice": 51.98,
                "merchantNetPriceFormatted": "$51.98",
                "priceFormatted": "$61.01",
                "minNoOfTravellersRequiredForPrice": 1
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 1
          }
        ]
      }
    ]
  },
  {
    "sortOrder": 2,
    "gradeCode": "TG2",
    "gradeTitle": "Double",
    "pricingMatrix": [
      {
        "sortOrder": 1,
        "pricingUnit": "per flight",
        "bookingDate": "2018-06-01",
        "ageBandPrices": [
          {
            "bandId": 1,
            "prices": [
              {
                "sortOrder": 1,
                "currencyCode": "USD",
                "price": 94.28,
                "merchantNetPrice": 80.33,
                "merchantNetPriceFormatted": "$80.33",
                "priceFormatted": "$94.28",
                "minNoOfTravellersRequiredForPrice": 1
              },
              {
                "sortOrder": 2,
                "currencyCode": "USD",
                "price": 94.28,
                "merchantNetPrice": 0,
                "merchantNetPriceFormatted": "$0.00",
                "priceFormatted": "$94.28",
                "minNoOfTravellersRequiredForPrice": 2
              }
            ],
            "sortOrder": 1,
            "minimumCountRequired": 1,
            "maximumCountRequired": 2
          }
        ]
      }
    ]
  }
]
```

**Interpretation**:

| Travelers | `gradeTitle` | Price per flight | Price per person |
|:----------:|-----------|:----------------:|:----------------:|
| 1 | Individual flight | $61.01 | $61.01 |
| 2 | Double | $94.28 | $47.14 |

### Per-plane pricing

**Request object**:

```javascript
{
  "productCode": "14876P5",
  "currencyCode": "USD",
  "month": "06",
  "year": "2019"
}
```

**Response snippet**:

```javascript
"pricingMatrix": [
  {
    "sortOrder": 1,
    "pricingUnit": "per plane",
    "bookingDate": "2019-01-02",
    "ageBandPrices": [
      {
        "bandId": 1,
        "prices": [
          {
            "sortOrder": 1,
            "currencyCode": "USD",
            "price": 433.03,
            "priceFormatted": "$433.03",
            "merchantNetPrice": 391.99,
            "merchantNetPriceFormatted": "$391.99",
            "minNoOfTravellersRequiredForPrice": 1
          },
          {
            "sortOrder": 2,
            "currencyCode": "USD",
            "price": 433.03,
            "priceFormatted": "$433.03",
            "merchantNetPrice": 0,
            "merchantNetPriceFormatted": "$0.00",
            "minNoOfTravellersRequiredForPrice": 2
          }
        ],
        "sortOrder": 1,
        "minimumCountRequired": 1,
        "maximumCountRequired": 3
      }
    ]
  }
]
```

**Interpretation**:
- "$433.03 per group of up to three people"
