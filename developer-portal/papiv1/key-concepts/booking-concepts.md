# Booking concepts

## Booking types - *on-request* and *freesale*

Bookings made with Viator can be either *freesale* (immediate confirmation) or *on-request*, which require us to confirm with our product supplier that the product has not sold out and is still available. This difference must be clearly communicated to the customer during the price check and on the *order summary* post-purchase page.

For *freesale* bookings, the voucher becomes available immediately, and the customer's credit card will be charged at the time of booking. For *on-request* bookings, confirmation will be sent to the customer within a timeframe supplied in the [/booking/calculateprice](../../../../openapi/reference/operation/bookingCalculateprice) and [/booking](../../../../openapi/reference/operation/bookingBook) service responses.

The customer's credit card will be charged once their *on-request* booking is confirmed.

##  bookingEngineId

`bookingEngineId` is a field returned in the responses from several endpoints documented in this manual. It is a **booking type specifier** indicating whether, when the product in question is booked, the booking will be `CONFIRMED` immediately or if it will remain `PENDING` even after the booking has been made, indicating that it is an on-request product.

`bookingEngineId` takes *one of* the following values:

  - `"FreesaleBE"` – the product will be confirmed immediately and the supplier will be sent a notification.

  - `"UnconditionalBE"` - the product will be confirmed immediately and the supplier will not be notified.

  - `"DeferredCRMBE"` - the product is an on-request product and the booking will not be confirmed immediately. The booking will remain with a `PENDING` status after it is made, to be confirmed by the supplier within the time specified in the `hoursConfirmed` field available in the booking response and post-booking services.

  - `"FreesaleOnRequestBE"` - The product is freesale up until a certain number of days before the travel date, after which it becomes on-request. It is then referred to as being within the *on-request period*. If a booking is made within the on-request period, the product can be considered to be an on-request product. Once the booking has been made, the `bookingEngineId` will change to either `"FreesaleOnRequestBE:OnRequest"` or `"FreesaleOnRequestBE:Freesold"` depending on the travel date and the on-request period.                      

## Tour grades

Products can have one or more *tour grades*. Each tour grade might represent a departure time or different tour option, such as additional meals, transport and so forth. If the tour grade code is `"DEFAULT"`, *do not* display this to the customer, simply hide the product's tour grade information.

## Language options

Many tours deliver a commentary in multiple languages using multilingual tour guides or with written or prerecorded information. Where available, the customer can preselect their preferred language option.

## Traveler mix (pax)

Some tour grades have defined traveler mixes used to price family passes; or, they might have special mixes for limited passenger tours, such as small buggies or weddings.

These traveler mixes are provided by the [/booking/availability/tourgrades](../../../../openapi/reference/operation/bookingAvailabilityTourgrades) service. You may need to display these to your customers so that they are able to understand why they can or cannot select a particular tour grade if there is a traveler mix mismatch.

## Pick-up location and hotel lists

Some products have pick-up and return shuttle bus services. For these tours, you will need the customer to supply a pick-up hotel, or they must select *live locally* or *hotel not yet booked* options.

Viator maintains pick-up hotel lists for many popular destinations. These lists are available for customers to select their pick-up location for various tours. For destinations without hotel lists, customers can enter the name of their hotel. If a customer's hotel is not listed, they should be able to enter a hotel name; however, pick-up may not be possible for that hotel.

## Lead traveler

Each tour booking requires a lead traveler to be identified. To identify the lead traveler in your request, set the `leadTraveller` flag to `true` in the traveler class.

## Booking questions

Some products have a list of one or more [booking questions](../../appendices/#booking-questions) that need to be asked. Some are mandatory. The question, a description, etc are provided in the product details object. The answers need to be included with the booking request.

## SSL/HTTPS

Calls to the [/booking/book](../../../../openapi/reference/operation/bookingBook) service *must* use a secure channel (https) as they contain credit card information.

## Promo codes

Viator can create promotional (promo) codes for discounts and other purposes. As it's unlikely for you to wish to support this feature, we recommend supplying `null` in the `promoCode` field and not including any customer-entered fields during the checkout process.

## Partner data

Partners can also supply additional information for their own internal purposes. These are attached to booking reports and other materials for use in allocating commissions to agents and so forth.