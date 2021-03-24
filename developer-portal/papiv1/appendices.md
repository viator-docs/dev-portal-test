# Appendices

## Update log

| Date | Description |
|------|-------------|
| 20 July 2020 | Added [Booking references](../key-concepts/booking-references) section |
| 14 July 2020 | Updated [Calculating prices](../workflows/calculating-prices) section |
| 2 June 2020 | Updated Postman collections and [Testing](../testing) section |
| 18 May 2020 | Added note regarding cancel codes to [Legacy merchant cancellation](../appendices/legacy-merchant-cancellation) section |
| 7 May 2020 | Upgraded search feature; enabled 'Try it Out' console |
| 10 Mar 2020 | Created new [Overview](../overview) section; removed 'Availability services' section, moving section contents into [Key Concepts](../key-concepts) section |

## Accept-Language header

The `Accept-Language` header parameter controls which language the natural language fields in the response from each endpoint will be translated into.

Note that you can only specify languages that have been configured for your API-key. Therefore, if you wish to access additional languages, you will need to contact your business development account manager.

| Language | Accept-Language parameter value |
|----------|-------|
| English | `en`, `en-US` |
| Danish | `da`, `da-DK` |
| Dutch | `nl`, `nl-NL` |
| Norwegian | `no`, `no-NO` |
| Spanish | `es`, `es-ES` |
| Swedish | `sv`, `sv-SE` |
| French | `fr`, `fr-FR` |
| Italian | `it`, `it-IT` |
| German | `de`, `de-DE` |
| Portuguese | `pt`, `pt-PT` |
| Japanese | `ja`, `ja-JP` |
| Chinese (simplified) | `zh-CN` |
| Chinese (traditional) | `zh-TW` |
| Korean | `ko`, `ko-KR` |

## Standard JSON fields

Every service returns a standard set of JSON fields at the end of the JSON response, which indicates if it was processed successfully by the API.

In addition to the `success` flag, you will also need to check the `errorMessage` values for the status of the response. 

An error-free response will have:

- `"success"`:`true`
- `"errorType"`:`null`
- `"errorMessage"`:`null`

### Example JSON - successful:

```javascript
{
  "data": [],
  "vmid": "321001",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2013-03-06T19:45:10+0000",
  "errorReference": null,
  "errorMessageText": null,
  "success": true,
  "totalCount": 114,
  "errorName": null
}
```

### Example JSON - unsuccessful:

```javascript
{
  "errorReference": "~5793740141815885188840666",
  "data": null,
  "dateStamp": "2013-09-09T11:29:48+0000",
  "errorType": "EXCEPTION",
  "errorMessage": ["* Additional questions missing\n"],
  "errorName": "ValidationException",
  "success": false,
  "totalCount": 1,
  "vmid": "221001",
  "errorMessageText": ["* Additional questions missing" ]
}
```

| Element | Type | Comments | To be viewed by customer | Required |
|---------|------|----------|:------------------------:|:--------:|
| `vmid` | string | The server id that processed the service | ❌ | ✅ |
| `errorMessage` | string | The error message in HTML | ❌ | ✅ |
| `errorType` | string | Type of error: EXCEPTION | ❌ | ✅ |
| `dateStamp` | string | timestamp of the response | ❌ | ✅ |
| `errorReference` | string | The error reference is logged for future reference | ❌ | ✅ |
| `errorMessageText` | string | The textual version of the error message | ✅<br /> (if an error has occurred) | ✅ |
| `success` | boolean | <ul><li>`true` if the API request was successful with no errors</li><li>`false` if an error was encountered</li></ul> | ❌ | ✅ |
| `totalCount` | integer | The number of results returned (minimum = `1`) | ✅<br /> (if displaying the number of results found in a search etc.) | ✅ |
| `errorName` | string | The name of the error type | ❌ | ✅ |

## Country codes
| Country code | Country |
|--------------|---------|
| AF | Afghanistan |
| AL | Albania |
| DZ | Algeria |
| AS | American Samoa |
| AD | Andorra |
| AO | Angola |
| AI | Anguilla |
| AQ | Antarctica |
| AG | Antigua and Barbuda |
| AR | Argentina |
| AM | Armenia |
| AW | Aruba |
| AU | Australia |
| AT | Austria |
| AZ | Azerbaijan |
| BS | Bahamas |
| BH | Bahrain |
| BD | Bangladesh |
| BB | Barbados |
| BY | Belarus |
| BE | Belgium |
| BZ | Belize |
| BJ | Benin |
| BM | Bermuda |
| BT | Bhutan |
| BO | Bolivia |
| BA | Bosnia Herzegovina |
| BW | Botswana |
| BR | Brazil |
| BN | Brunei |
| BG | Bulgaria |
| BF | Burkina Faso |
| BI | Burundi |
| KH | Cambodia |
| CM | Cameroon |
| CA | Canada |
| CV | Cape Verde |
| KY | Cayman Islands |
| CF | Central Africa |
| TD | Chad |
| CL | Chile |
| CN | China |
| CX | Christmas Island |
| CC | Cocos (Keeling) Islands |
| CO | Colombia |
| KM | Comoros |
| CK | Cook Islands |
| CR | Costa Rica |
| CI | Cote D'Ivoire |
| HR | Croatia |
| CY | Cyprus |
| CZ | Czech Republic |
| DK | Denmark |
| DJ | Djibouti |
| DM | Dominica |
| DO | Dominican Republic |
| EC | Ecuador |
| EG | Egypt |
| SV | El Salvador |
| GQ | Equatorial Guinea |
| ER | Eritrea |
| EE | Estonia |
| ET | Ethiopia |
| FK | Falkland Island |
| FO | Faroe Islands |
| FJ | Fiji |
| FI | Finland |
| FR | France |
| GF | French Guiana |
| PF | French Polynesia |
| GA | Gabon |
| GM | Gambia |
| GE | Georgia |
| DE | Germany |
| GH | Ghana |
| GI | Gibraltar |
| GR | Greece |
| GL | Greenland |
| GD | Grenada |
| GP | Guadeloupe |
| GU | Guam |
| GT | Guatemala |
| GN | Guinea |
| GW | Guinea Bissau |
| GY | Guyana |
| HT | Haiti |
| HN | Honduras |
| HK | Hong Kong |
| HU | Hungary |
| IS | Iceland |
| IN | India |
| ID | Indonesia |
| IQ | Iraq |
| IE | Ireland |
| IL | Israel |
| IT | Italy |
| JM | Jamaica |
| JP | Japan |
| JO | Jordan |
| KZ | Kazakhstan |
| KE | Kenya |
| KI | Kiribati |
| KW | Kuwait |
| KG | Kyrgyzstan |
| LA | Lao People's Democratic Republic |
| LV | Latvia |
| LB | Lebanon |
| LS | Lesotho |
| LR | Liberia |
| LY | Libyan Arab Jamahiriya |
| LI | Liechtenstein |
| LT | Lithuania |
| LU | Luxembourg |
| MO | Macau |
| MK | Macedonia |
| MG | Madagascar |
| MW | Malawi |
| MY | Malaysia |
| MV | Maldives |
| ML | Mali |
| MT | Malta |
| MQ | Martinique |
| MR | Mauritania |
| MU | Mauritius |
| YT | Mayotte |
| MX | Mexico |
| FM | Micronesia |
| MD | Moldova |
| MC | Monaco |
| MN | Mongolia |
| MS | Monserrat |
| MA | Morocco |
| MZ | Mozambique |
| NA | Namibia |
| NR | Nauru |
| NP | Nepal |
| NL | Netherlands |
| AN | Netherlands Antilles |
| KN | Nevis- St Kitts |
| NC | New Caledonia |
| NZ | New Zealand |
| NI | Nicaragua |
| NE | Niger |
| NG | Nigeria |
| NU | Niue |
| NF | Norfolk Island |
| KP | North Korea |
| MP | Northern Mariana Islands |
| NO | Norway |
| OM | Oman |
| PK | Pakistan |
| PW | Palau |
| PS | Palestinian Territory, Occupied |
| PA | Panama |
| PG | Papua New Guinea |
| PY | Paraguay |
| PE | Peru |
| PH | Philippines |
| PN | Pitcairn |
| PL | Poland |
| PT | Portugal |
| PR | Puerto Rico |
| QA | Qatar |
| RE | Reunion |
| RO | Romania |
| RU | Russian Federation |
| RW | Rwanda |
| SH | Saint Helena |
| LC | Saint Lucia |
| SM | San Marino |
| ST | Sao Tome and Principe |
| SA | Saudi Arabia |
| SN | Senegal |
| YU | Serbia and Montenegro |
| SC | Seychelles |
| SL | Sierra Leone |
| SG | Singapore |
| SK | Slovakia |
| SI | Slovenia |
| SB | Solomon Islands |
| SO | Somalia |
| ZA | South Africa |
| KR | South Korea |
| ES | Spain |
| LK | Sri Lanka |
| PM | St Pierre Miquelon |
| VC | St Vincent and Grenadines |
| SR | Suriname |
| SZ | Swaziland |
| SE | Sweden |
| CH | Switzerland |
| SY | Syria |
| TW | Taiwan |
| TJ | Tajikistan |
| TZ | Tanzania |
| TH | Thailand |
| TL | Timor-Leste |
| TG | Togo |
| TK | Tokelau |
| TO | Tonga |
| TT | Trinidad and Tobago |
| TN | Tunisia |
| TR | Turkey |
| TM | Turkmenistan |
| TC | Turks and Caicos Islands |
| TV | Tuvalu |
| UG | Uganda |
| UA | Ukraine |
| AE | United Arab Emirates |
| GB | United Kingdom |
| UY | Uruguay |
| UM | US Minor Outlying Islands |
| US | United States of America |
| UZ | Uzbekistan |
| VU | Vanuatu |
| VE | Venezuela |
| VN | Vietnam |
| VG | Virgin Islands-British |
| VI | Virgin Islands-US |
| WF | Wallis and Futuna Islands |
| WS | Western Samoa |
| YE | Yemen Republic |
| ZM | Zambia |
| ZW | Zimbabwe |

## US state codes
| State code | State |
|------------|-------|
| AL | Alabama |
| AK | Alaska |
| AZ | Arizona |
| AR | Arkansas |
| CA | California |
| CO | Colorado |
| CT | Connecticut |
| DE | Delaware |
| DC | District of Columbia |
| FL | Florida |
| GA | Georgia |
| HI | Hawaii |
| ID | Idaho |
| IL | Illinois |
| IN | Indiana |
| IA | Iowa |
| KS | Kansas |
| KY | Kentucky |
| LA | Louisiana |
| ME | Maine |
| MD | Maryland |
| MA | Massachusetts |
| MI | Michigan |
| MN | Minnesota |
| MS | Mississippi |
| MO | Missouri |
| MT | Montana |
| NE | Nebraska |
| NV | Nevada |
| NH | New Hampshire |
| NJ | New Jersey |
| NM | New Mexico |
| NY | New York |
| NC | North Carolina |
| ND | North Dakota |
| OH | Ohio |
| OK | Oklahoma |
| OR | Oregon |
| PA | Pennsylvania |
| RI | Rhode Island |
| SC | South Carolina |
| SD | South Dakota |
| TN | Tennessee |
| TX | Texas |
| UT | Utah |
| VT | Vermont |
| VA | Virginia |
| WA | Washington |
| WV | West Virginia |
| WI | Wisconsin |
| WY | Wyoming |

## Canadian provinces

| Code | Province |
|------|----------|
| Alberta | Alberta |
| British Columbia | British Columbia |
| Manitoba | Manitoba |
| New Brunswick | New Brunswick |
| Newfoundland and Labrador | Newfoundland |
| Northwest Territories | Northwest Territories |
| Nova Scotia | Nova Scotia |
| Nunavut | Nunavut |
| Ontario | Ontario |
| Prince Edward Island | Prince Edward Island |
| Quebec | Quebec |
| Saskatchewan | Saskatchewan |
| Yukon | Yukon Territory |

## Australian states

| Code | State |
|------|-------|
| ACT | Australian Capital Territory |
| NSW | New South Wales |
| NT | Northern Territory |
| QLD | Queensland |
| SA | South Australia |
| TAS | Tasmania |
| VIC | Victoria |
| WA | Western Australia |

## bookingStatus field values and meanings

| Field: `type` | Field: `status` | Meaning |
|-------|:-----:|---------|
| `"WAITING"` | `0` | This item has not been booked. Part of an unfinished itinerary. |
| `"CONFIRMED"` | `1` | This item has been successfully booked |
| `"UNAVAILABLE"` | `2` | This item has been had an availability check, that came back false. |
| `"PENDING"` | `3` | This item has begun booking, but has paused in a deferred booking engine |
| `"FAILED"` | `4` | A merchant partner with a freesale-only API-key has attempted to book an on-request product |
| `"CANCELLED"` | `5` | This item has successfully been cancelled. |
| `"EXPIRED"` | `6` | This item is now expired. |
| `"AMENDED"` | `7` | This item has been amended after booking. |
| `"PENDING_AMEND"` | `8` | This item has a pending amendment which can be cancelled. |
| `"REJECTED"` | `12` | Viator only |
| `"ON_HOLD"` | `13` | Viator only |

## Supported currency codes

Supported currency codes for merchant partners:

| Currency code | Currency |
|---------------|----------|
| USD | US dollar |
| GBP | British pound |
| EUR | Euro |
| AUD | Australian dollar |

**Note:** Partners will be billed in the currency of the booking.

## Viator API error codes

| Error code | Services | Error message | Description |
|------------|----------|---------------|-------------|
| ADDRESS_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "You have not entered an address. Please enter your address information." | `ccAddress1` was empty |
| ADDRESS\_SIZE\_EXCEEDED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Billing address is restricted to 50 characters long." | `address` field was longer than 51 characters |
| ATTRIBUTE\_NOT\_FOUND | [/product](../../../../openapi/reference/operation/product) |   |   |
| AGE\_BAND\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) |  | a `bandId` has been submitted that does not correspond to any `bandId` available for the tour grade in question |
| BOOKING\_QUESTIONS\_MISSING | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Additional questions missing" | one or more required [booking questions](#section/Appendices/Booking-questions) are missing in the booking request |
| CARD\_EXPIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook)  |   | submitted credit card details corresponding to an expired card  |
| CITY\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "You have not entered a city in the address section. Please enter your city." | address is required in the credit card section |
| CITY\_SIZE\_EXCEEDED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "City is restricted to 40 characters long." | city name submitted was more than 40 characters long |
| COUNTRY\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "You have not entered a country. Please enter your country." | no country was submitted in the booking request  |
| CREDIT\_CARD\_DECLINED | [/booking/book](../../../../openapi/reference/operation/bookingBook) |   | credit card used for booking was declined by the payment processor  |
| CREDIT\_CARD\_EXPIRY\_DATE\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "The expiration date for your credit card is not formatted properly. Please verify and re-enter the expiration date." | incorrectly-formatted credit card expiration date was submitted  |
| CREDIT\_CARD\_HOLDER\_NAME\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) |  | credit card holder's name was invalid, perhaps due to the inclusion of invalid characters |
| CREDIT\_CARD\_NUMBER\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Please verify and re-enter the credit card details, or use a different credit card" | invalid characters in credit card number |
| CREDIT\_CARD\_NUMBER\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Credit card number is required" | credit card number was omitted |
| CREDIT\_CARD\_SECURITY\_NUMBER\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "The card security number you entered for your credit card is invalid. It must contain 3 digits (or 4 with American Express cards). Please re-enter the card security number." | incorrect CCV code submitted |
| CREDIT\_CARD\_SECURITY\_NUMBER\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Credit card security number is required" | CCV was not provided |
| DEMO\_BOOKING\_WITH\_REAL\_CARD | [/booking/pricingmatrix](../../../../openapi/reference/operation/bookingPricingmatrix) |  | `demo` is `true`, but credit card details are real |
| DISTRIBUTOR\_REFERENCE\_MISMATCH | [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking); [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) | "The distributor reference associated with this itinerary does not match the one provided." | attempt to retrieve a booking with an `itineraryId` or `itemId` and `distributorRef`, but the reference doesn't match the one saved in the itinerary |
| EMAIL\_ADDRESS\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Your email address format is invalid" | email address is formatted incorrectly  |
| EMAIL\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Email is required" | email address is missing in the booking request |
| FIRST\_NAME\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "You have entered a name for the credit-card holder that is not valid. Please verify and re-enter the name of the credit card holder." | first name is formatted incorrectly or contains invalid characters in the booking request - string length must be > 1 and must not contain the following characters: <>%;"(), |
| FIRST\_NAME\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "First name of credit card details is required" | no first name specified |
| FIRST\_NAME\_SIZE\_EXCEEDED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "First name of credit card details is restricted to 30 characters long" | first name exceeds 30 characters |
| INTERNAL\_ERROR | *any* | *any* | the API itself has experienced an unexpected error |
| LAST\_NAME\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) |  "You have entered a name for the credit-card holder that is not valid. Please verify and re-enter the name of the credit card holder." | last name is formatted incorrectly in the booking request – string length must be > 1 and must not contain the following characters: <>%;"(), |
| LAST\_NAME\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Last name of credit card details is required" | no last name supplied in the `ccname` field of the `ccPayDetail` object |
| LAST\_NAME\_SIZE\_EXCEEDED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Last name of credit card details is restricted to 35 characters long" | the surname in the `ccname` field of the `ccPayDetail` object must not exceed 35 characters in length |
| LEAD\_TRAVELLER\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "A traveler needs to be selected as lead traveler." | one traveller object within the `travellers` array in the booking request needs to have `leadTraveller` set to `true` |
| PAYMENT\_AMOUNTS\_CHANGED |  [/booking/book](../../../../openapi/reference/operation/bookingBook) | e.g. "PAYMENT\_AMOUNTS\_CHANGED: HKD 2213.20 (was HKD 2210.26)" | This error indicates that the exchange rate was updated while the booking was being made. Refresh the product's pricing information and retry the booking.  |
| PAYMENT\_CURRENCY\_MISMATCH | [/booking/book](../../../../openapi/reference/operation/bookingBook) |   |   |
| PAYMENT\_ENCRYPTION\_ERROR | [/booking/book](../../../../openapi/reference/operation/bookingBook)  |   | Viator-only internal error - retry booking request |
| PAYMENT\_INTERNAL\_ERROR | [/booking/book](../../../../openapi/reference/operation//bookingBook)  |   | Viator-only internal error - retry booking request |
| PAYMENT\_LIMIT\_REACHED | [/booking/book](../../../../openapi/reference/operation/bookingBook)  |   | Viator-only internal error - retry booking request |
| PAYMENT\_REJECTED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | | triggered when expiry: `"01/2018"` and card number: `"4539791001730106"` were submitted |
| POSTCODE\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "You have not entered a zip code / post code. Please enter your zip code / post code.", | `ccaddressZip` was empty |
| POSTCODE\_SIZE\_EXCEEDED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Zip code / post code is restricted to 10 characters long." | `ccaddressZip` was more than 10 characters in length |
| PRICING\_DATA\_MISSING |   |   | Viator-only internal error - retry booking request |
| PRODUCT\_TOUR\_GRADE\_UNKNOWN | [/booking/pricingmatrix](../../../../openapi/reference/operation/bookingPricingmatrix) | Unknown tour grade: <TOUR\_GRADE> for product | an invalid tour grade code was submitted |
| PRODUCT\_UNAVAILABLE | [/booking/book](../../../../openapi/reference/operation/bookingBook)  |   | Viator-only internal error - retry booking request |
| REFUND\_REJECTED |   |   |  Viator-only internal error - retry booking request |
| STATE\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | | `ccaddressState` is required for this billing request |
| STATE\_SIZE\_EXCEEDED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | | `ccaddressState` must be fewer than 35 characters long |
| TOUR\_NOT\_AVAILABLE | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "We're sorry, the following tour you are trying to book is sold out and no longer available" | the tour is not available on the requested date |
| TOUR\_GONE | [/product](../../../../openapi/reference/operation/product) | "We're sorry, we cannot find the tour, activity or attraction you are looking for" | no product corresponding to the supplied details was found |
| TOUR\_NOT\_AVAILABLE\_BETWEEN\_DATES | | | |
| TOUR\_NOT\_FOUND | [/product](../../../../openapi/reference/operation/product) | "We're sorry, we cannot find the tour, activity or attraction you are looking for" | no product corresponding to the supplied details was found |
| TRAVELLER\_COUNT\_EXCEEDED\_MAX\_LIMIT |   |   | number of travellers in the booking request was greater than the limit for the product being booked |
| TRAVELLER\_FIRST\_NAME\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "First name of traveler 1 must only contain alphabetical characters" | non-alphabetical characters were used in the traveller's first name |
| TRAVELLER\_FIRST\_NAME\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "First name of traveler 1 is required" | `firstname` in the `booker` object was omitted |
| TRAVELLER\_LAST\_NAME\_INVALID | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Last name of traveler 1 should contain alphabet only" | `surname` in the `booker` object contained non-alphabetical characters |
| TRAVELLER\_LAST\_NAME\_REQUIRED | [/booking/book](../../../../openapi/reference/operation/bookingBook) | "Last name of traveler 1 is required" | `surname` in the `booker` object was omitted |
| TRAVELLER\_MISMATCH |  [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades) |   | the `bandId` is not available for the selected tour grade; or, the product does not support the number of travelers requested |
| UNKNOWN\_ERROR | *any* | *any* | the API reports this error when the exception from the underlying system (e.g. booking server) is not recognized |
| UNKNOWN\_PAYMENT\_METHOD |   |   |   |
| UNSUPPORTED\_CARD |   |   | `cctype` is not one of `"Visa"`, `"Mastercard"` or `"Amex"` |

## Booking questions

Example product codes were valid at the time of writing. If you find that any of these product codes are invalid or do not include the relevant booking question, please [inform us about it via email](mailto:dpsupport@viator.com).

| Id | stringQuestionId | title | subtitle | message | example product |
|:----------:|-------------|-------|----------|----------|-----|
| 1 | `dateOfBirth_dob` | Date of Birth | (e.g. 20 October 1970) | Enter your date of birth. | 100009P2 |
| 2 | `heights_passengerHeights` | Passenger Heights | (eg. 5'2, 158cm etc) | For safety reasons you must enter the height of all passengers. Please indicate inches or centimetres. | 100009P1 |
| 3 | `passport_expiry` | Passport Expiry Date | (e.g. 15 September 2015. If multiple passengers, separate each entry e.g. 01 July 2012, 31 May 2014) | Enter passport expiry date for all passengers | 100014P10 |
| 4 | `passport_nationality` | Passport Nationality | (e.g. United States of America. If multiple passengers, separate each entry e.g. Australia, China) | Enter country of issue of passport for all passengers | 100014P10 |
| 5 | `passport_passportNo` | Passport Number | (e.g. 0123456789. If multiple passengers, separate each entry e.g. 0123456789, 9876543210) | Enter passport number for all passengers | 100014P10 |
| 6 | N/A | N/A | N/A | N/A | N/A |
| 7 | `transfer_air_arrival_airline` | Arrival Airline | (e.g. United, British Airways, Qantas, etc) | Enter the name of your airline. | 100006P15 |
| 8 | `transfer_air_arrival_flightNo` | Arrival Flight No | (e.g. UA 864, BA 923, QA 233, etc) | Enter your flight number. | 100006P15 |
| 9 | `transfer_air_departure_airline` | Departure Airline | (e.g. United, British Airways, Qantas, etc) | Enter the name of your airline. | 100006P17 |
| 10 | `transfer_air_departure_flightNo` | Departure Flight No | (e.g. UA 864, BA 923, QA 233, etc) | Enter your flight number. | 100006P17 |
| 11 | `transfer_arrival_dropOff` | Drop Off Location | (e.g. 1234 Cedar Way, Brooklyn, NY 00123) | Enter the address for drop off. | 100006P15 |
| 12 | `transfer_arrival_time` | Arrival Time | (eg. 8pm, 20:30 etc) | Enter your arrival time. Please indicate AM/PM or use the 24-hour clock. | 100006P15 |
| 13 | `transfer_departure_date` | Departure date | (e.g. 15 September 2015) | Enter your departure date. | 100006P15 |
| 14 | `transfer_departure_pickUp` | Pick up Location | (e.g. 1234 Cedar Way, Brooklyn, NY 00123) | Enter the address for pick up. | 100006P17 |
| 15 | `transfer_departure_time` | Departure Time | (eg. 8pm, 20:30 etc) | Enter your departure time. Please indicate AM/PM or use the 24-hour clock. | 100006P17 |
| 16 | `transfer_port_arrival_time` | Disembarkation Time | (eg. 8pm, 20:30 etc) | Enter your disembarkation time. Please indicate AM/PM or use the 24-hour clock. | 100014P14 |
| 17 | `transfer_port_cruiseShip` | Cruise Ship | (e.g. Brilliance of the Seas, etc) | Enter your cruise ship. | 100014P14 |
| 18 | `transfer_port_departure_time` | Boarding Time | (eg. 8pm, 20:30 etc) | Enter your boarding time. Please indicate AM/PM or use the 24-hour clock. | 100014P4 |
| 19 | `transfer_rail_arrival_line` | Arrival Rail Line | (e.g. Amtrak, etc) | Enter the name of the rail provider. | 100006P15 |
| 20 | `transfer_rail_arrival_station` | Arrival Rail Station | (e.g. Central Station, etc) | Enter name of arrival and/or departure station. | 100006P15 |
| 21 | `transfer_rail_departure_line` | Departure Rail Line | (e.g. Amtrak, etc) | Enter the name of the rail provider. | 100014P10 |
| 22 | `transfer_rail_departure_station` | Departure Rail Station | (e.g. Central Station, etc) | Enter name of arrival and/or departure station. | 100014P10 |
| 23 | `weights_passengerWeights` | Passenger Weights | (e.g. 127 pounds, 145 kilos, etc) | For safety reasons you must enter the weight of <b>all</b> passengers. Please indicate pounds or kilos. | 100111P12 |

## Legacy merchant cancellation

**Note:** This functionality has been replaced by the [/bookings/cancel-reasons](../../../../openapi/reference/operation/cancellationReasons), [/bookings/{booking-reference}/cancel-quote](../../../../openapi/reference/operation/bookingQuote) and [/bookings/{booking-reference}/cancel](../../../../openapi/reference/operation/cancelBooking) endpoints.

### Requirements for cancellations

- To successfully cancel a booking via the [/merchant/cancellation](../../../../openapi/reference/operation/merchantCancellation) service, you must include the itinerary item to cancel (`itemId`). 

- `itineraryItemId` and `itineraryId` need to match the `distributorRef` and `distributorItemRef`, so these four values must also be included in the request body. 

- You must also include a `cancelCode` - a number corresponding to the reason for cancellation. You can use the <a href="#suggested-cancellation-codes">suggested cancel codes</a> shown in the table below.

<mark>**Note**: Post-travel cancellations **will not be processed** unless a cancel code of `62` or `66` is passed in the `cancelCode` parameter.</mark>
### The [/merchant/cancellation](../../../../openapi/reference/operation/merchantCancellation) service:

#### Description of JSON request parameters for the [/merchant/cancellation](../../../../openapi/reference/operation/merchantCancellation) service:

| Parameter | Type | Comments | Required |
|-----------|------|----------|:--------:|
| `itineraryId` | integer | Viator itinerary reference number | ✅ |
| `distributorRef` | string | Merchant partner's itinerary reference for booking | ✅ |
| `cancelItems` | array | Array of item to cancel in itinerary | ✅ |
| `itemId` | integer | Viator `itemId` of item to cancel in itinerary | ✅ |
| `distributorItemRef` | string | Merchant partner's itinerary item (booking) reference | ✅ |
| `cancelCode` | string | A number indicating the reason for cancelling the booking. A list of <a href="#suggested-cancellation-codes">suggested cancel codes</a> is shown in the table below. | ✅ |
| `cancelDescription` | string | Natural-language reason for cancellation. A reason **must** be provided if a `cancelCode` of `'62'` or `'66'` is passed. | ✅ for `cancelCode` `'62'` or `'66'`; otherwise ❌ |

#### Example [/merchant/cancellation](../../../../openapi/reference/operation/merchantCancellation) request:

In this request, we wish to cancel the booking identified by the following:

| Parameter | Value |
|-----------|-------|
| `itneraryId` | `12345655` |
| `distributorRef` | `"Jdp122"` |
| `itemId` | `330056` |
| `distributorItemRef` | `"JdpItin001"` |
| `cancelCode` | `"82"` (Honest mistake - incorrect purchase) |

This is accomplished as follows:

**API Service**

```html
POST /merchant/cancellation
```

**Request body**

```javascript
{
  "itineraryId": 1234655,
  "distributorRef": "Jdp122",
  "cancelItems": [
  {
    "itemId": 330056,
    "distributorItemRef": "JdpItin001",
    "cancelCode": "82"
  }]
}
```

#### Example response
```javascript
{
  "data": {
    "itineraryId": 1234655,
    "cancelItems": [
      {
        "cancellationResponseStatusCode": "Confirmed",
        "cancellationResponseDescription": "No further action required",
        "itemId": 330056,
        "distributorItemRef": "JdpItin001"
      }],
    "distributorRef": "Jdp122"
  },
  "vmid": "221002",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2013-03-21T14:28:08+0000",
  "errorReference": null,
  "errorMessageText": null,
  "success": true,
  "totalCount": 1,
  "errorName": null
}
```

### <a name="suggested-cancellation-codes"></a>Suggested cancellation codes:

| cancelcode | meaning |
|:-:|:-|
| `'00'` | Testing (use for test cancellations) |
| `'51'` | Flight cancellation affecting customer |
| `'52'` | Flight schedule change unacceptable to customer |
| `'53'` | Death of the customer or a member of their immediate family |
| `'54'` | Jury duty/court summons affecting customer |
| `'56'` | Medical emergency/hospitalization involving the customer or their immediate family |
| `'57'` | Customer is required for military service |
| `'58'` | National disaster (insurrection, terrorism, war) affecting the customer |
| `'59'` | Natural disaster (earthquake, fire, flood) affecting the customer |
| `'62'` | **Post-travel cancellation**: the product was cancelled by the supplier and the traveller was not given sufficient notice |
| `'63'` | Transport strike/labor dispute affecting customer |
| `'66'` | **Post-travel cancellation**: the product was not cancelled, but the customer was dissatisfied with the product |
| `'71'` | Credit card fraud |
| `'72'` | Car segment cancellation affecting customer |
| `'73'` | Package segment cancellation affecting customer |
| `'74'` | Hotel segment cancellation affecting customer |
| `'77'` | Re-book |
| `'78'` | Duplicate purchase |
| `'82'` | Honest mistake (incorrect purchase) |
| `'87'` | Non-refundable cancellation more than 24 hours prior to travel |
| `'88'` | Non-refundable cancellation less than 24 hours prior to travel |
| `'98'` | Customer service/technical support response outside time limit |
| `'99'` | Duplicate processing |

### Cancellation errors

If the cancellation was **not** successful, you will receive an error response.

#### Example error response

```javascript
{
  "data": {
    "itineraryId": "3331605", 
    "cancelItems": [{
      "cancellationResponseStatusCode": "Error.ItineraryUnknown", 
      "cancellationResponseDescription": "Please double check the details or contact..."
      "itemId": "600088255",
      "distributorItemRef": "ItinItemRef012"
    }],
    "distributorRef": null 
  },
  "vmid": "221002",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2013-03-21T14:43:38+0000", 
  "errorReference": null, 
  "errorMessageText": null,
  "success": true, 
  "totalCount": 1, 
  "errorName": null
}
```

### <a name="cancellation-response-status-codes-and-their-meanings"></a>Cancellation response status codes and their meanings

| `cancellationResponseStatusCode` | Meaning | Action |
|----------------------------------|----------|--------|
| `"Confirmed"` | The request to cancel and refund the item has been accepted and processed | No further action is required. |
| `"Pending"` | Confirmation of the request to cancel and refund the item is pending. This only applies when a `cancelCode` is `'62'` or `'66'` was sent and the booking was in a 'pending' state. | No action required. Partner will be contacted when a decision to confirm/reject has been made by the supplier. |
| `"Rejected"` | The cancellation request was denied | No action required. The item cannot be cancelled. |
| `"Error.ItemUnknown"` | Item not found | Double-check `itemId`. Contact Viator Customer Service for more information if required. |
| `"Error.ItineraryUnknown"` | Itinerary not found | Double-check `itineraryId`. Contact Viator Customer Service for more information if required. |
| `"Error.MultipleRequests"` | Cancellation request contains multiple requests | Submit **only one** item per cancellation request. |
| `"Error.NoCancellationCodeOrDescription"` | Invalid `cancelCode` | `cancelCode` is invalid – ensure it is **two** digits long |
| `"Error.Unknown"` | An undefined error has occurred | Double-check the `distributorRef` and `distributorItemRef`. If the error is still occurring, [contact the Viator partner support team](mailto:dpsupport@viator.com). |

### Resubmitting a cancellation request

If the same cancellation request is sent more than once, Viator will respond with the last known response.