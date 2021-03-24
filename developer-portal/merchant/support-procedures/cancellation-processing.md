# Cancellation processing
## Cancellation terms – pre-travel

You are required to establish the cancellation terms that will apply to their customers. These terms are to be set with respect to our cancellation terms; i.e., yours may be *more* strict than ours if necessary, but *not less* strict.

API partners are responsible for adhering to the cancellation policies for each product and for informing their customers about the cancellation terms that apply. The Viator website allows customers to self-manage their bookings. As this is accomplished via the API, you will need to build out the requisite workflows in your own implementation to actually support this cancellation functionality.

If you have not yet implemented any cancellation functionality – or were unaware that it existed – please see the technical discussion on <a href="/partner-api/merchant/technical/#section/Common-workflows-and-data-validation/Cancellation-API-workflow" target="_blank" data-wm-adjusted="done">cancelling a booking</a>.

## Cancellation terms – duplicate bookings

Please note that if you make a duplicate booking and the supplier does not approve the refund, we will not be held responsible and will not refund the cost of the ticket.

## Cancellation terms – post-travel

From time to time, you may need request cancellation of a product *after* your customer has completed their travel. This might be due to a service issue (e.g., the tour could not operate due to weather or other operational issues) or a complaint (e.g., the customer did not feel they received what was advertised).

To submit a post-travel cancellation request, you should submit the cancellation via an acceptable method (auto-cancel or cancellation form) but with the code(s) below to identify the issue:

- **Reason code 62** (service complaint - denied trip add-on service) 
    - cancellations due to a customer complaint or service issue where the supplier did not cancel the tour directly.
- **Reason code 66** (trip add on supplier cancellation) 
    - cancellations where a supplier cancelled the tour, or the tour did not operate, and the traveler was not notified in advance.

For all post-travel cancellation requests, we will need a detailed description of the issue. In all cases (except known service interruptions – weather, etc.), Viator will first verify the issue genuinely exists and then seek authorization from the product supplier. Once a decision has been made, we will notify your customer services department with resolution information. You then need to advise the traveler directly.