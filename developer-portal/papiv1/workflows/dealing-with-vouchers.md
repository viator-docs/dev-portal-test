# Dealing with vouchers

The [/booking/voucher](../../../../openapi/reference/operation/bookingVoucher) service retrieves details for a complete itinerary or a single itinerary item. The data is returned as HTML that can be wrapped in a header/footer.

## Sample URL parameters

`leadLastName=DP&itemId=600033670`

or

`voucherKey=3299307:93c7f36a56b18ba1068787ba7fb7988da5c8ad08db77604110141ff21498603e:600033670`

## Key concepts

### `voucherKey`
- Use either the `voucherKey` or the three separate parameters.
- If `voucherKey` is provided as well as other parameters, then the `voucherKey` overrides the other parameters.
- The `voucherKey` is obtained from [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings) or in the [/booking/book](../../../../openapi/reference/operation/bookingBook) response object when a booking is made.

### `fullHTML`
This is an optional parameter: 
- If `true`, the full HTML (including `<!DOCTYPE>`, `<html>` and `<head>` tags) will be returned.
- If `false`, an HTML `<div>` element will be returned.
- The default for this parameter is `false`

### `mobileVoucher`
- Optional parameter. Defaults to `true`. If `true`, the mobile (cut down) voucher HTML is returned; otherwise, the full voucher HTML is returned and `fullHTML` is ignored
- This field should only be enabled for products that have a `voucherOption` of `"VOUCHER_E"` (electronic voucher). Do not enable `mobileVouchers` for paper vouchers (`voucherOption` of `"VOUCHER_PAPER_ONLY"`) as no barcode is returned.
- The voucher information is available in the responses for:
  - [/product](../../../../openapi/reference/operation/product)
  - [/booking/book](../../../../openapi/reference/operation/bookingBook)
  - [/booking/pastbooking](../../../../openapi/reference/operation/bookingPastbooking)
  - [/booking/mybookings](../../../../openapi/reference/operation/bookingMybookings)
- Voucher information is also displayed under the **Redemption Info** heading in the response from this service.

**Example response object** ([/booking/voucher](../../../../openapi/reference/operation/bookingVoucher))

```javascript
{
  "data": "<div style=\'line-height: 1.5;font-family:\'Arial\',\'Helvetica\',\'Verdana\',sans-serif; font-size: 12px; padding: 0 10px; border-bottom: 1pxsolid #CAE2EA;\'><h2 style=\'font-size:16px;font-weight:bold;margin:0.5em 0;padding:0;\'>San FranciscoBay Sunset Catamaran Cruise &reg;</h2><h2 style=\'font-size:16px;font-weight:bold;margin:0.5em0;padding:0;\'>SAMPLE ONLY</h2><ul style=\'margin:0 0 1em 1em; padding:0;\'> <li><strong>Date:</strong>Friday April 13, 2012 </li><li><strong>Time:</strong><strong>2011:</strong><br><ul><li><strong>Nov. 6 to Nov. 27</strong>: 4:00pm (Fri., Sat. &amp; Sun)</li></ul><p><strong>2012:</strong></p><ul><li><strong>March 2 to March 10:</strong>&nbsp; 5:00 pm (Fri., Sat. &amp; Sun)</li><li><strong>March 11 to April 15:</strong> 6:00pm Daily</li><li><strong>April 16 to May 20:</strong> 6:30 pm Daily</li><li><strong>May 21 to July 22:</strong> 7:00 pm Daily</li><li><strong>July 23 to Aug 26: </strong>6:30 pm Daily</li><li><strong>Aug 27 to Sept 23:</strong>6:00 pm Daily</li><li><strong>Sept. 24 to Nov. 3:</strong> 5:30 pm Daily</li><li><strong>Nov 4 to Dec 2:</strong> 4:00pm (Friday, Sat., &amp; Sun.)</li></ul><p>Please arrive 30 minutes prior to cruise departure.</li></ul> <ul style=\'margin:0 0 1em 1em; padding:0;\'> <li><strong>Lead Traveler: </strong> jos dp</li><li><strong>Number of Travelers: </strong> 1 Adult</li> <li><strong>Booking Reference: </strong>VIATOR600033672</li><li><strong>Product Code: </strong>2316SUN</li><li><strong>Confirmation Details:</strong>SUN </li> <li><strong>Location </strong><div><p>Pier 39</p></div><div></div><div>When you get to Pier 39, stand on the sidewalk &amp; look towards the water, do NOT go down the center wherethe shops are, but take the left OUTSIDE walkway. Go towards the sea lions &amp; look for a gate with the letter J on it</div></li></ul><h3 style=\'font-size:14px;font-weight:bold;margin:0.5em 0;padding:0;\'> Redemption Info</h3><ul style=\'margin:0 0 1em 1em; padding:0;\'> <li>You can present either a paper or an electronic voucherfor this activity. </li> </ul> <h3 style=\'font-size:14px;font-weight:bold;margin:0.5em 0;padding:0;\'>Important</h3> <ul><li>Your local contact is Adventure Cat Sailing Charters on +1 800 498 4228.</li><li>Please note: You mustreconfirm directly with Adventure Cat Sailing Charters at <ul> <li>Locally on 415 777 1630</li></ul> at least 24 Hour(s)prior to your tour/activity date. If you are not arriving within the specified timeframe, please contact Adventure CatSailing Charters prior to your travels, or immediately upon arrival at your destination.</li></ul><ul><li>Duringthe months of March, April and November, the weather in San Francisco can be unpredictable and sailings are subject tocancellation or rescheduling. Please ensure that you call the operator 1 day prior to sailing to confirm your tour</li></ul><h3 style=\'font-size:14px;font-weight:bold;margin:0.5em 0;padding:0;\'>Inclusions</h3><ul><li>1.5-hour Sunset Cruise</li><li>Light hors d\'oeuvres</li><li>Two complimentary drinks</li></ul><h3 style=\'font-size:14px;font-weight:bold;margin:0.5em 0;padding:0;\'>Terms and Conditions </h3> Read our completeTerms & Conditions for information on cancellations, date changes and other modifications to this confirmed reservation. </div><!-- end of voucher_item --></div>",
  "vmid": "221001",
  "errorMessage": null,
  "errorType": null,
  "dateStamp": "2012-04-13T10:40:47+0000",
  "success": true,
  "errorReference": null,
  "errorMessageText": null,
  "totalCount": 1,
  "errorName": null
}
```
