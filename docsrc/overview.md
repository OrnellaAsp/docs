# Overview and responsibilities

An overview of the payment lifecycle can be viewed in the following diagram:

<img src="../flowchart.png" alt="Flowchart of payment cycle" style="width: 550px;"/>

## Detailed overview

<ol>
<li>
<b>User loads customer website</b> The payment journey will always start from the customer website - the user may click a link to donate, go to a checkout after inserting some items in a basket, or any other action that requires a payment to be taken.
</li>
<li>
<b>User enters details unrelated to payments</b> <i>This step is optional.</i> Asperato will, by default, collect all data required to process a payment. However, many customers wish to collect other data that will also populate the Salesforce record - eg. "How did you hear about us" questions. If this type of data is required, it should be collected at this point.
</li>
<li>
<b>Payment record is created in Salesforce</b> The Asperato ONE package provides a webservice to create a default, empty payment record that can then be used to take a payment. Where payment options do not need to be customised, this is the simplest approach, and the responsibility of creating the record lies with the Asperato ONE package.<br>
However, where a greater level of customisation is required, the payment record can be created via any other means in Salesforce. This custom route allows intelligent customisation of the payment page based on any data available - eg. if the amount is greater than $1000, you may choose to only allow credit cards, if the payor is in the UK you may wish to only allow direct debit, etc.
</li>
<li>
<b>Customisable Asperato payment page is shown</b> A URL is generated from the payment record after its creation, and this URL will show the payment page for this particular payment. By default, Asperato provide a fully working, customisable and responsive payment page that opens in a new window to take the payment. However, the template may be customised extensively if required (see Template Documentation) and may also be inlined into your own website via an iframe for a smoother user journey.
</li>
<li>
<b>User enters details relating to payment</b> The user enters all the payment details on the securely hosted Asperato payment page.
</li>
<li>
<b>User submits payment details to Asperato</b> The user presses the submit button on the payment page, which sends the payment details to the Asperato server.
</li>
<li>
<b>Asperato communicates with PSPs (Payment Service Providers)</b> Asperato receives the payment details and then communicates with the PSP that is set up for the given customer and payment route (Stripe, Sagepay, GoCardless, Paypal, etc.)
</li>
<li>
<b>Asperato updates the relevant payment record in Salesforce</b> After the PSP has responded, Asperato will update the payment object with the relevant details. If the transaction has failed then Asperato will report the reason for failure; if the transaction is successful Asperato will update the payment with the payor details. If there is an error in Salesforce that causes the record not to update for any reason (and Salesforce reports this error), we will email details of the issue to a pre-configured email address or mailing list.
</li>
<li>
<b>Asperato displays the exit or error page</b> If the transaction was successful, Asperato will display an "exit page", otherwise will display an "error page". Similarly to the payment page, both the exit and error page are provided by default, but can also be extensively customised.
</li>
</ol>