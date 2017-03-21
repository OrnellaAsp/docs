# Gateway Keys

Asperato will require keys to connect to a user's payment service provider. Unfortunately, retrieving these keys is not consistent across providers. You should be able to ask customers to find the keys using the following instructions.

## Authorize.NET
If you don't know what these values are you can create new ones, but beware that this will expire the old ones and will there affect anything that is communicating with Authroize.net via their API.

Go to `Account > Settings > API Credentials & Keys`

## Braintree
The information Asperato needs can be obtained from the Braintree Dashboard.

For test/sandbox this is at <a href="https://sandbox.braintreegateway.com/login">https://sandbox.braintreegateway.com/login</a>

For live/production this is at <a href="https://www.braintreegateway.com/login">https://www.braintreegateway.com/login</a>

On the top of the dashboard screen there is an Account option, and under that appears a `My User` option when you hover over it.

In the My Account screen, towards the bottom, is a section entitled "API Keys, Tokenization Keys, Encryption Keys".  Click on "View Authorizations".

You need to create a new API Key.  Click on the button marked "Generate New API Key".  This will create a new key.  In the line that appears on the screen there is a View link under the heading "Private Key". Click on that.

Asperato then needs the values of the fields marked:
- Public Key
- Private Key
- Merchant ID

## Worldpay Online
You need to log onto the <a href="https://online.worldpay.com/login">Worldpay Online Dashboard</a>.
- Ensure you are in TEST mode.
- On the top right of the screen click on "SETTINGS".
- Select the tab entitled "API Keys".
- We need the Service key & Client key.
