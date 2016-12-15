# Payment template

The standard payment template is provided by Asperato and will look similar to the following:

![Asperato standard payment template](unmodified_template.png "Asperato standard template")

To obtain a copy of the standard paypage for modification, please email support@asperato.com. Most users only wish to make a few standard modifications to the template - this is the recommended approach. However, it is possible to complete redesign the template if required.

# Standard modifications

* **Changing the company name**
Open `style.css`, and find the following section:

		.companyname::before {
			content: "MyCompany";
		}
		
	Change the content to your own company name.
	
	If you are using GoCardless Pro, and have your own SUN, then you also need to find and change the following sections:
	
		.ddcompanyname::before {
			/* Change this to your company name */
			content: "GoCardless Ltd";
		}
		
		.creditorid::before {
			/* Change this to your creditor id (only required if using SEPA direct debit) */
			content: "GB18ZZZSDDRB0S00275069";
		}

		.ddcompanyaddress::before {
			/* Change this to your company address, telephone number and email address respectively */
			content: "338-346 Goswell Road, London, EC1V 7LQ, United Kingdom, 020 7183 8674, help@gocardless.com";
		}
		
	If you are using GoCardless standard, you must leave these fields as they are. If you are not using direct debit, you can ignore these.

* **Changing the logo**
Just replace the `logo.png` file in the template directory with one of your own. The recommended size is `380x120`, but you can experiment to find a size that works well.

* **Changing the font**
Open `style.css`, and find the following section:


        body {
        	margin: 0;
	        text-align: center;
	        background-color: white;
	        font-family: karla;
        }

    Change `karla` to whatever font family you wish to use on the template.

* **Changing the background colour**
As above, Open `style.css`, and find the following section:


        body {
        	margin: 0;
	        text-align: center;
	        background-color: white;
	        font-family: karla;
        }

    Change `white` to whatever background colour you wish to use on the template.

* **Changing the submit button colour**
Open `style.css`, and find the following section:

		.submitbutton {
			...
			background-color: #5092DA;
			...
		}
		
	Change `#5092DA' to whatever background colour you wish to use for the submit button.

* **Removing the cancel link**
Open `style.css`, and find the following section:


         .cancelbutton {
			 /* Uncomment if a cancel button isn't required on the payment page */
			 /*display: none;*/
		 }

    Uncomment the `display: none;` as directed (remove the `/*` and `*/` either side.)

* **Removing the "pay as company" link**
Open `style.css`, and find the following section:


        .label.right.payascompanylink {
			/* Uncomment to hide the "pay as company" link */
			/*display: none;*/
		}

    Uncomment the `display: none;` as directed (remove the `/*` and `*/` either side.)

# Extensive modifications

The paypage as provided can be modified to provide it with an entirely custom look - both the HTML markup and the CSS may be changed.

However, extensively modifying the template can be a lot of work, and you can end up needing to re-implement many of the features the original template provides:

* Javascript based card type identification from number (IIN range checker)
* Start date and issue number fields dynamically displayed (based on card number) when required
* Validation of user input, including Luhn algorithm on card number
* Third party validation of bank details prior to submission (via Ajax)
* UK postcode lookup
* Responsive to mobile devices
* Payment options set dynamically within Salesforce (Card, Direct debit or Paypal)
* Can switch between taking a payment or updating an authorisation
* Fully compatible with all modern browsers

Asperato are unable to provide support for templates with extensive modifications.

If you do choose to extensively modify the payment page, then you must read this document thoroughly and adhere to the guidelines.

When your modifications are complete, you must [validate the document](https://validator.w3.org/). Unless there is a very good reason, **Asperato will not allow invalid HTML to be used on payment pages.**

## ID / class attributes

When modifying the document, please keep the class and ID attributes that are already present, as changing them will break form submission and validation. (Additional class names may be used.)

## Additional JS

Additional Javascript is fine, but please place it in a separate file (where possible) rather than altering the provided "script.js" file. We need to manually check the Javascript to make sure it is not malicious in any way, so placing it in a separate file will speed up this process.

## Endpoint & Method

* The method must be `POST`. Other HTTP methods (such as `GET`) are not supported.

* The endpoint (action parameter) must either be `/PMWeb2`, or `/PMWeb2X`.

    * `PMWeb2X` will display a gear animation while the payment is processing, but cannot be used with a hosted payment option (such as GoCardless standard or Paypal);

    * `PMWeb2` will not display the gear animation.

## Onload Javascript

Immediately after the opening body tag, the following must be left in place:

`<!-- ONLOAD HERE -->`

This will provide a hook for injecting the Javascript we need to dynamically modify the paypage (based on data collected from Salesforce.)

## Postcode lookup

You may wish to include a postcode lookup in your page - we have this facility available as a simple JSON endpoint to which you can fire an AJAX request.

**Endpoint:** `/Asperato/asp/PMPostcode`

* This will be [https://test.protectedpayments.net/Asperato/asp/PMPostcode](https://test.protectedpayments.net/Asperato/asp/PMPostcode) for test and [https://live.protectedpayments.net/Asperato/asp/PMPostcode](https://test.protectedpayments.net/Asperato/asp/PMPostcode) for live.

* Do not specify the full URL in your Javascript, as the same origin policy will then break the functionality when moving from test to live.

**Method: **`GET`

**Parameters:**

* `pc` - this is the postcode to look up. It should be all in lower case and contain no spaces.

* `session` - this is the value of the `DLsession` parameter on the form.

The response will be given as a JSON array.

For example:

* `GET` [https://test.protectedpayments.net/Asperato/asp/PMPostcode?pc=HD80pq&session=1133557799](https://test.protectedpayments.net/Asperato/asp/PMPostcode?pc=HD80pq&session=1133557799)

Response:

*["22 Penistone Road, , , , Kirkburton, Huddersfield, West Yorkshire","24 Penistone Road, , , , Kirkburton, Huddersfield, West Yorkshire","24a Penistone Road, , , , Kirkburton, Huddersfield, West Yorkshire","26a Penistone Road, , , , Kirkburton, Huddersfield, West Yorkshire","28 Penistone Road, , , , Kirkburton, Huddersfield, West Yorkshire","30 Penistone Road, , , , Kirkburton, Huddersfield, West Yorkshire","32 Penistone Road, , , , Kirkburton, Huddersfield, West Yorkshire","Shepley Springs Ltd, Brookfield Mills, Penistone Road, , Kirkburton, Huddersfield, West Yorkshire","Spring Grove Fisheries, 26 Penistone Road, , , Kirkburton, Huddersfield, West Yorkshire","Spring Grove Tavern, 20 Penistone Road, , , Kirkburton, Huddersfield, West Yorkshire","The Foxglove, 36a Penistone Road, , , Kirkburton, Huddersfield, West Yorkshire"]*

The response can then be used to fill a HTML selection box or similar, and the selected value can be used to populate the required fields on the form.

## Field names

In all cases here, both the `id` and `name` attributes of the element must be set to the name given. The attribute value is case sensitive.

Taking `DLemail` as an example:

`<input type="text" name="DLemail" id="DLemail">`

### Required fields

The following fields must be specified on the form regardless of payment method or type:

#### DLsession

This is an internally generated session number *(this field should be set to "type=hidden" and will be automatically populated from Salesforce.)*

#### DLpayMode

This is an internally generated session number *(this field should be set to "type=hidden" and will be automatically populated from Salesforce.)*

#### DLpayType

This is an internally generated session number *(this field should be set to "type=hidden" and will be automatically populated from Salesforce.)*

#### DLFrequency

The frequency of payment. You may wish to restrict the values to those provided in the "LabelFrequencyOptions" field.

Possible values:

* `ONE` (single payment)
* `DLY` (daily payment)
* `WKL` (weekly payment)
* `MON` (monthly payment)
* `QTL` (quarterly payment)
* `SAN` (semi-annual payment)
* `ANN` (annual payment)

#### DLamount

The amount to charge. One of DLamount or DLauthAmount must be provided, depending on whether this is a payment template or an authorisation template.

#### DLauthAmount

The amount to authorise. One of DLamount or DLauthAmount must be provided, depending on whether this is a payment template or an authorisation template.

#### DLemail

The email address of the customer.

#### LabelFrequencyOptions

The frequency options passed from Salesforce. *(this field should be set to "type=hidden" and will be automatically populated from Salesforce. Its “id” attribute must also match the field name.)*

#### LabelPaymentOptions

The payment options passed from Salesforce *(this field should be set to "type=hidden" and will be automatically populated from Salesforce. Its “id” attribute must also match the field name.)*

You may wish to use this field to restrict the payment options - i.e. hide the option to pay via direct debit if it is set to "card" or vica versa.

### Required card fields

The following fields must (only) be specified if a debit or credit card payment is being made.

#### DLcardNumber

The long number on the front of the card.

#### DLcardType

Possible values:

* Visa

* Mastercard

* Amex

* Solo

* Switch

* Maestro

* JCB

#### DLcnp

Set to "jdi" if cardholder is not present, otherwise do not specify this parameter.

#### DLcurrency

The ISO currency code, eg. "GBP" for British pounds.

#### DLexpiryDateMonth

The two digit expiry date month of the card.

#### DLexpiryDateYear

The last two digits of the year the card expires.

#### DLcv2

The 3 digit security code on the back of the card (or the 4 digit code on the top right of the card for Amex.)

### Optional card fields

These fields may be required - they are not for the majority of cards, but some (such as Solo, Maestro and Switch) may need them, so if you expect customers to pay with these cards they should be included.

#### DLstartDateMonth

The two digit start date month of the card

#### DLstartDateYear

The last two digits of the start date year.

#### DLissueNumber

The issue number of the card (often "01" where it exists.)

### Required Direct Debit / ACH fields

These fields are only required if making a direct debit or ACH payment.

#### DLaccountName

The full name of the account holder.

#### DLaccountNumber

The bank account number.

#### DLsc

The sort code (direct debit) or routing number (ACH).

#### DLaccountType

Only necessary for ACH - the account type. Usually "checking" or “savings”.

### Address fields

If AVS (address) checking is enabled on your card gateway, then you will need some of these fields. The exact fields you require depends on your gateway settings, but often just the first line of the address (`DLaddress1`) and the postcode (`DLpostcode`) are required.

For direct debit and ACH payments, the title, forename, lastname, first line of the address, city and postcode are always required.

#### DLtitle

The salutation or title.

#### DLforename

The first name.

#### DLlastname

The last name.

#### DLaddress1

The first line of the address.

#### DLaddress2

The second line of the address.

#### DLcity

The town or city of the address

#### DLcounty

The county or state of the address

#### DLcountry

The country of the address

#### DLpostcode

The postcode, or zip code of the address.

# Pre-populating the template
The template will be pre-populated with values for the above parameters if values are provided as part of the URL string.

For example, if the normal URL to the payment page is:

<https://test.protectedpayments.net/Asperato/asp/PMWeb1?pmRef=73&campaignRef=431>

Then the email can be pre-populated with the following URL:

<https://test.protectedpayments.net/Asperato/asp/PMWeb1?pmRef=73&campaignRef=431&DLemail=test@asperato.com>

# Direct Debit Legal Requirements

While the look and feel of both the payment and exit screens can be changed, the following rules must be adhered to if UK direct debit or SEPA payments are being used. These requirements are mandatory and non-negotiable as they are required under UK law.

The example payment page template includes all these requirements, so if you are just modifying the colours, fonts, and logo in use then you need not worry about the contents of this document. If you are developing a template with a radically different look and feel, we will need to send these to GoCardless for verification before they can be used live (this process can typically take a couple of weeks, depending on how many changes are required.)

## Payment page requirements
**Account name field must be called “Account name”**
The label for the account name field must not be changed - it must also be presented in a logical block next to the account number and sort code. (In particular, it cannot be changed just to “name”.)

**Retain confirmation that only one user is required to authorise payments.**
The check box that asks whether multiple signatures are needed to set up a direct debit on this account must be retained.

**Confirm customer details before submission.**
The customer’s bank account details (including account name, account number and sort code) must be relayed back to the customer in a confirmation dialog, with the opportunity to amend these details, before the form is submitted.

**Provide your contact details**
You need to provide the name, address, phone number and email address of your payment provider somewhere on the payment page, usually in the footer. Note that if you are using GoCardless standard, this needs to be kept as GoCardless’ details (included in the sample template.) If you are using GoCardless Pro with your own SUN, then these need to be your own details.

**Include a link to the direct debit guarantee**
A link must be clearly visible on the page that points to the direct debit guarantee - the wording of the guarantee needs to stay exactly as it is in the sample template, aside from swapping out the company name. As previously, this needs to be left as GoCardless if GoCardless standard is being used, and changed to your company name if you are using GoCardless Pro with your own SUN.

## Confirmation page requirements
**Show confirmation of setup**
Confirmation that the direct debit was set up correctly must be displayed.

**Provide the mandate as a PDF link**
You need to provide a link to the Direct Debit mandate as a PDF.

**Confirm that the customer will receive an email**
You need to send the customer an email confirming that the direct debit was setup successfully, along with a link to their mandate (as above.) You need to state that this email will arrive in 3 business days.

**Confirm the name on the customer’s bank statement**
You need to include “The name on your bank statement will be X” on your confirmation page. X will be “GoCardless” for GoCardless standard customers, and your company name for GoCardless Pro customers using their own SUN.

## Error page requirements
**Confirm that the Direct Debit was not set up**
Confirm to the user that an error occurred, and as a result the direct debit was not set up correctly.