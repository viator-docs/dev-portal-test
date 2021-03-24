# Booking cancellation checks in the production environment

**Action**: Demonstrate that your booking cancellation mechanism is functioning correctly by confirming **two** bookings in the live/production environment **and** successfully cancel those bookings. One booking must be for a product with a **standard** cancellation policy and the other must be for a product with an **all sales final** policy. Access to the live environment for this test will be granted once all other checks are verified and the certification process is completed.

**Note**: 

- When making bookings in the live production environment for this test, `demo` must be set to `false` in the request.
- Products with a **standard** cancellation policy should be booked for dates sufficiently far in the futures that cancellation can be accomplished without penalty.
- Bookings for non-refundable (all sales final) products will be manually refunded by the Viator team after the check is completed.
- It is mandatory to complete this check in order to go live.

## Product examples:

| Cancellation policy type | Example products |
|--------------------------|------------------|
| Standard | 54969P13, 2065CWL, 23043P2, 177350P25 |
| All sales final | 6479UY1DAY, 161943P27, 63607P2, 190211P3, 6787P5 |