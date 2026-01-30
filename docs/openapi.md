# Trustap API

The Trustap API is a REST-based API for managing user data and
transactions for Trustap users.

Note that, like Stripe, all API `price` and `charge` fields expect
amounts to be provided in a currency's smallest unit. See [the Stripe
documentation](https://stripe.com/docs/currencies#zero-decimal) for more
details.

API Keys
--------

Some endpoints such as `/charge` endpoints can be accessed directly
using an API key. API keys are provided as the "username" portion of a
basic auth authentication. For example, the Trustap fee for a new online
transaction can be determined using the following cURL request with an
API key:

``` bash
curl -u '<api_key>': 'https://dev.stage.trustap.com/api/v1/charge?currency=eur&price=1234'
```

Deprecation
-----------

Deprecated endpoints are tagged with the word `DEPRECATED` at the start
of their description. The deprecation message will describe what
endpoints/approach should be used instead.

Note that deprecated endpoints will not be removed from the current
version of the API, but their use is discouraged. Furthermore, migrating
to later major versions of the API will be made easier by avoiding
deprecated endpoints.

Production
----------

This documentation references the staging environment of the Trustap
API, which is available at `https://dev.stage.trustap.com` and uses the
`trustap-stage` realm during authentication. This environment can be
used for implementing and testing features using test credit cards, bank
accounts, etc.

When all features using Trustap have been implemented and tested
sufficiently, the production environment of the Trustap API can be used
to process actual currency. This environment is available at
`https://dev.trustap.com` and uses the `trustap` realm during
authentication.

Note that only your client ID will be the same in the staging and
production Trustap API environments; your client secret and API key will
be different in the two environments.


Version: 0.1.0

## Servers

```
https://dev.stage.trustap.com/api/v1
```

## Security

### APIKey

Type: http
Scheme: basic

### OAuth2

Type: oauth2

## Download OpenAPI description

[Trustap API](https://docs.trustap.com/_bundle/apis/openapi.yaml)

## Buyers and Sellers

Users represent users of the platform.


### Create a new guest user

 - [POST /guest_users](https://docs.trustap.com/apis/openapi/buyers-and-sellers/users.createguestuser.md): This endpoint creates guest user which can be used as the buyer or the seller.
If guest user is used as the seller in a transaction, they need to be created with
country_code parameter correctly set. This is important because of currencies
they can use in their transactions.
tos_acceptance is the parameter which is important for disputes/chargeback and 
represent the timestamp when Trustap ToS were accepted https://www.trustap.com/terms.

### Get the balance for the current user in each currency

 - [GET /me/balances](https://docs.trustap.com/apis/openapi/buyers-and-sellers/users.getbalances.md)

### Get the status enabled/disabled of user's features

 - [GET /me/features](https://docs.trustap.com/apis/openapi/buyers-and-sellers/users.getuserfeatures.md)

### Allow users to turn instant payouts feature on/off

 - [POST /me/features/instant_payouts](https://docs.trustap.com/apis/openapi/buyers-and-sellers/users.setinstantpayouts.md)

## Clients

Clients represent partner applications.


### Get supported registration countries for the current client

 - [GET /client/supported_registration_countries](https://docs.trustap.com/apis/openapi/clients/client.getsupportedregistrationcountries.md)

### Get timelines for the given client

 - [GET /clients/{client_id}/timelines](https://docs.trustap.com/apis/openapi/clients/basic_client.gettimelinesforclient.md)

### Get timelines for the given client

 - [GET /p2p/clients/{client_id}/timelines](https://docs.trustap.com/apis/openapi/clients/p2p_client.gettimelinesforclient.md)

## Personal

Personal endpoints are used for managing personal details for users, for
"Know Your Customer" requirements.


### Create an account session and retrieve the client secret for it

 - [GET /me/account_session](https://docs.trustap.com/apis/openapi/personal/personal.getaccountsession.md)

### Get debit account details

 - [GET /me/debit_account](https://docs.trustap.com/apis/openapi/personal/personal.getdebitaccount.md)

### Set debit account details

 - [POST /me/debit_account](https://docs.trustap.com/apis/openapi/personal/personal.setdebitaccount.md)

### Get instant payout balance

 - [GET /me/instant_payout_balance](https://docs.trustap.com/apis/openapi/personal/personal.getinstantpayoutbalance.md)

### Get the payout attempts that have occurred for this account

 - [GET /me/payout_attempts](https://docs.trustap.com/apis/openapi/personal/personal.getpayoutattempts.md): Payout attempts are returned in reverse chronological order -
the first attempt in the returned array will be the most recent
payout attempt.

### Get the verification status for the front side of the secondary identity document

 - [GET /me/personal/additional_identity_document/verification_status](https://docs.trustap.com/apis/openapi/personal/getadditionalidentitydocumentverificationstatus.md)

### Get the verification status for the back side of the secondary identity document

 - [GET /me/personal/additional_identity_document_back/verification_status](https://docs.trustap.com/apis/openapi/personal/getadditionalidentitydocumentbackverificationstatus.md)

### Get personal details for the local user

 - [GET /me/personal/details](https://docs.trustap.com/apis/openapi/personal/personal.getdetails.md)

### Set personal details for the local user

 - [PATCH /me/personal/details](https://docs.trustap.com/apis/openapi/personal/personal.setdetails.md)

### Get the verification status for the front side of the primary identity document

 - [GET /me/personal/identity_document/verification_status](https://docs.trustap.com/apis/openapi/personal/getidentitydocumentverificationstatus.md)

### Get the verification status for the back side of the primary identity document

 - [GET /me/personal/identity_document_back/verification_status](https://docs.trustap.com/apis/openapi/personal/getidentitydocumentbackverificationstatus.md)

### Get the details for the Stripe publishable key the user is linked to

 - [GET /me/personal/stripe_publishable_key](https://docs.trustap.com/apis/openapi/personal/getstripepublishablekeyforuser.md): Returns the Publishable Key for the Stripe Platform
which the user is linked to.

### Get the status of a user's ability to accept payouts

 - [GET /me/profile/payout_status](https://docs.trustap.com/apis/openapi/personal/getprofilepayoutstatus.md): This endpoint requires
the user identified by the Trustap-User header to have granted the profile
scope to the client that is performing the request. Offline access is allowed for this endpoint when the user has
granted the profile scope to the client
that is performing the request.

### Add a new payout method to the current account using a payment method ID

 - [POST /me/stripe_financial_connections/add_account](https://docs.trustap.com/apis/openapi/personal/personal.addstripefinancialconnectionsaccount.md)

### Get the ownership details from the given SetupIntent using Stripe Financial Connections

 - [GET /me/stripe_financial_connections/ownership](https://docs.trustap.com/apis/openapi/personal/personal.getstripefinancialconnectionsownership.md)

### Get a SetupIntent ID and secret for starting a Stripe Financial
Connections session

 - [POST /me/stripe_financial_connections/prepare_account](https://docs.trustap.com/apis/openapi/personal/personal.preparestripefinancialconnectionsaccount.md)

### Create verification method and message if not available

 - [GET /me/verification_method](https://docs.trustap.com/apis/openapi/personal/personal.getverificationmethod.md)

### Create verification session and retrieve client secret for it

 - [GET /me/verification_session](https://docs.trustap.com/apis/openapi/personal/personal.getverificationsession.md): Offline access is allowed for this endpoint when the user has granted the profile scope to the client that is performing the request.

### Create verification session for native apps that use Stripe native SDK

 - [GET /me/verification_session_native](https://docs.trustap.com/apis/openapi/personal/personal.getverificationsessionnative.md): Creates verification session for native apps that use Stripe native SDK, 
and returns it's Verification Session ID with appropriate Ephemeral Key.
Offline access is allowed for this endpoint when the user has
granted the profile scope to the client that is performing the request.

### Get verification status of the user

 - [GET /me/verification_status](https://docs.trustap.com/apis/openapi/personal/personal.getverificationstatus.md): Offline access is allowed for this endpoint when the user has granted the profile scope to the client that is performing the request.

## Online Transactions

Online transactions are used when the goods are delivered using registered
mail.


### Get multiple online transactions by their IDs

 - [GET /batch/transactions](https://docs.trustap.com/apis/openapi/online-transactions/basic.gettransactionsbyids.md): This endpoint takes a required ids parameter that contains a
comma-separated list of transaction IDs, and returns the
transactions corresponding to those IDs. A maximum of 50 IDs may
be provided, results are returned in the order specified in
ids, and duplicates are preserved. Transactions that couldn't
be found will be returned as null values in the result array.
The following example request results in the 200 OK response
described below, assuming that the transaction with ID 1990
couldn't be found:

 http
https://dev.stage.trustap.com/api/v1/batch/transactions?ids=1309,609,1990,609

### Get online transactions for the logged-in user

 - [GET /me/transactions](https://docs.trustap.com/apis/openapi/online-transactions/basic.gettransactions.md): Transactions are currently sorted by ID.

### Create a new online transaction

 - [POST /me/transactions](https://docs.trustap.com/apis/openapi/online-transactions/basic.createtransaction.md): A request to this endpoint must be preceded by a call to
/charge with the price of the goods specified in price.

### Create a new online transaction with both users

 - [POST /me/transactions/create_and_join](https://docs.trustap.com/apis/openapi/online-transactions/basic.createandjointransaction.md): This endpoint is an optimised call that allows a transaction to
be created and joined in a single request. It requires the user
identified by join_user_id to have granted the
basic_tx:offline_create_join scope to the client that is
performing the request.

### Create a new online transaction with both users

 - [POST /me/transactions/create_with_guest_user](https://docs.trustap.com/apis/openapi/online-transactions/basic.createtransactionwithguestuser.md): This endpoint is an optimised call that allows a transaction to
be created and joined in a single request. It requires the
online user to have granted the basic_tx:offline_create_join scope
to the client that is performing the request.
This endpoint allows creating a transaction with both buyer and seller
as guest or full users, or one party to be guest user and the other full user.

### Get an online transaction using a join code

 - [GET /transactions](https://docs.trustap.com/apis/openapi/online-transactions/basic.gettransactionbyjoincode.md)

### Join an online transaction using a join code

 - [PUT /transactions](https://docs.trustap.com/apis/openapi/online-transactions/basic.jointransaction.md): A transaction can't be joined using its ID, because only the
buyer and seller of a transaction can access a transaction using
its ID.

### Get an online transaction by its ID

 - [GET /transactions/{transaction_id}](https://docs.trustap.com/apis/openapi/online-transactions/basic.gettransaction.md)

### Claim online transaction on behalf of the buyer

 - [POST /transactions/{transaction_id}/claim_for_buyer](https://docs.trustap.com/apis/openapi/online-transactions/basic.claimtransactionforbuyer.md): This endpoint enables a client to claim an online transaction on behalf of a buyer.
This endpoint is only accessible for a client.

### Claim online transactions on behalf of the seller

 - [POST /transactions/{transaction_id}/claim_for_seller](https://docs.trustap.com/apis/openapi/online-transactions/basic.claimtransactionforseller.md): This endpoint enables a client to claim an online transaction on behalf of a seller.
This endpoint is only accessible for a client.

### Submit a detailed order issue for this online transaction

 - [POST /transactions/{transaction_id}/submit_order_issue](https://docs.trustap.com/apis/openapi/online-transactions/basic.submitorderissue.md)

### Claim for an online transaction as buyer given a claim secret

 - [POST /transactions_by_claim_secret/{secret}/claim_as_buyer](https://docs.trustap.com/apis/openapi/online-transactions/basic.claimasbuyer.md): This endpoint allows a full user to claim for a transaction
as buyer given a claim secret.

### Claim an Online transaction as the seller

 - [POST /transactions_by_claim_secret/{secret}/claim_as_seller](https://docs.trustap.com/apis/openapi/online-transactions/basic.claimtransactionasseller.md): This endpoint allows a full user to claim a transaction
as seller given a claim secret.

## Online Payment

### Get the Trustap fee for an online transaction

 - [GET /charge](https://docs.trustap.com/apis/openapi/online-payment/basic.getcharge.md): This returns the Trustap fee, in the currency's smallest unit,
for a transaction involving goods with the supplied price. See
[the Stripe
documentation](https://stripe.com/docs/currencies#zero-decimal)
for more details.

### Update the description, currency, price and/or charge of an online transaction

 - [PATCH /transactions/{transaction_id}](https://docs.trustap.com/apis/openapi/online-payment/updatetransaction.md)

### Enable the seller to accept payment for an online transaction

 - [POST /transactions/{transaction_id}/accept_payment](https://docs.trustap.com/apis/openapi/online-payment/basic.acceptpayment.md): This endpoint lets the seller accept payment and is accessible only if the seller
has granted a feature require_seller_acceptance.
Offline access is allowed for this endpoint when the user has
granted the basic_tx:offline_accept_payment scope to the client
that is performing the request.

### Enable payment acceptance for the seller using Trustap-User for an online transaction

 - [POST /transactions/{transaction_id}/accept_payment_with_guest_seller](https://docs.trustap.com/apis/openapi/online-payment/basic.acceptpaymentwithguestseller.md): This endpoint allows payment acceptance for the seller specified in the
header as Trustap-User.

### Get bank transfer details for an online transaction which payment method is bank_transfer

 - [GET /transactions/{transaction_id}/bank_transfer_details](https://docs.trustap.com/apis/openapi/online-payment/basic.getbanktransferdetails.md)

### Get the Trustap fee for an online transaction

 - [GET /transactions/{transaction_id}/charge](https://docs.trustap.com/apis/openapi/online-payment/basic.getchargefortransaction.md): This returns the Trustap fee, in the currency's smallest unit,
for a transaction involving goods with the supplied price. See
[the Stripe
documentation](https://stripe.com/docs/currencies#zero-decimal)
for more details.

### Set the payment method to be used for this online transaction

 - [POST /transactions/{transaction_id}/set_payment_method](https://docs.trustap.com/apis/openapi/online-payment/basic.setpaymentmethodfortransaction.md): Different payment methods incur different rates, so the payment
method to be used for a transaction must be set before the
payment is made. This requires retrieving the charge associated
with the payment method using
GET /transactions/{transaction_id}/charge, and passing the
generated charge as a parameter to this endpoint.

## Online Shipping

### Get the list of Shippo shipping rates available

 - [POST /shippo_shipping_rates](https://docs.trustap.com/apis/openapi/online-shipping/basic.getshipposhippingrates.md): This returns the list of Shippo shipping rates available
for the addreses provided i.e sender's address and recipient's
address.

### Get the supported carriers

 - [GET /supported_carriers](https://docs.trustap.com/apis/openapi/online-shipping/getsupportedcarriers.md): This returns the carriers that are currently supported for
automated tracking in the online model. The carrier name
should be shown to users but the code should be submitted to
the /track endpoint when submitting tracking details.
Likewise, when showing a transaction to the user, the carrier
field stored with the transaction should be used to index these
carriers and show the human-readable name of the carrier to the
user, if this index is found.

### Confirm delivery for this online transaction

 - [POST /transactions/{transaction_id}/confirm_delivery](https://docs.trustap.com/apis/openapi/online-shipping/basic.confirmdelivery.md): This endpoint allows the buyer to manually confirm the delivery of the
item in the case that the state of the transaction was not updated
asynchronously by Trustap.

### Confirm delivery for this online transaction

 - [POST /transactions/{transaction_id}/confirm_delivery_with_guest_buyer](https://docs.trustap.com/apis/openapi/online-shipping/basic.confirmdeliverywithguestbuyer.md): This endpoint allows the client to manually confirm the delivery
on behalf of the guest buyer specified in the header as Trustap-User.

### Extend tracking details submission deadline for this online transaction

 - [POST /transactions/{transaction_id}/extend_tracking_deadline](https://docs.trustap.com/apis/openapi/online-shipping/basic.extendtrackingdeadlinefortransaction.md): This endpoint allows the buyer to extend the tracking details
submission deadline for the seller.

### Generate a shipment label for this online transaction's parcel

 - [POST /transactions/{transaction_id}/generate_shipment_label](https://docs.trustap.com/apis/openapi/online-shipping/basic.generateshipmentlabel.md): This endpoint returns a PDF containing the label for the
transaction's parcel. Both sender and recipient details
have to be submitted before making a call to this endpoint.

### Get the shipping details of the buyer from this online transaction

 - [GET /transactions/{transaction_id}/shipping_details](https://docs.trustap.com/apis/openapi/online-shipping/basic.getshippingdetailsfromtransaction.md)

### Post the shipping details of the buyer from this online transaction

 - [POST /transactions/{transaction_id}/shipping_details](https://docs.trustap.com/apis/openapi/online-shipping/basic.setshippingdetails.md)

### Set the sender's or recipient's address for Shippo shipment

 - [POST /transactions/{transaction_id}/shippo_address](https://docs.trustap.com/apis/openapi/online-shipping/basic.setshippoaddress.md)

### Send a customs declaration for this online transaction's shipment

 - [POST /transactions/{transaction_id}/shippo_customs_declaration](https://docs.trustap.com/apis/openapi/online-shipping/basic.setshippocustomsdeclaration.md)

### Set Shippo parcel details for this online transaction

 - [POST /transactions/{transaction_id}/shippo_parcel_details](https://docs.trustap.com/apis/openapi/online-shipping/basic.setshippoparceldetails.md)

### Get Shippo generated label details for this online transaction

 - [GET /transactions/{transaction_id}/shippo_shipping_label](https://docs.trustap.com/apis/openapi/online-shipping/basic.getshipposhippinglabel.md)

### Set Shippo shipping rate for this online transaction

 - [POST /transactions/{transaction_id}/shippo_shipping_rate](https://docs.trustap.com/apis/openapi/online-shipping/basic.setshipposhippingrate.md): Set Shippo shipping rate for the transaction.
This rate will be used to purchase the Shippo label once the
transaction is paid. Shipment ID can be provided as an optional parameter,
depending on the flow which is used to obtain it.

### Get Shippo shipping rates based on the sender's and recipient's address and parcel details

 - [GET /transactions/{transaction_id}/shippo_shipping_rates](https://docs.trustap.com/apis/openapi/online-shipping/basic.getshipposhippingratesfortransaction.md)

### Set postal tracking details for this online transaction

 - [POST /transactions/{transaction_id}/track](https://docs.trustap.com/apis/openapi/online-shipping/basic.tracktransaction.md): After the tracking information has been submitted then the state
of the transaction will be updated asynchronously by Trustap
once the delivery of the item has been confirmed, at which point
the delivered field of the transaction will be set.
Offline access is allowed for this endpoint when the user has
granted the basic_tx:offline_track scope to the client
that is performing the request.

### Set postal tracking details for this online transaction

 - [POST /transactions/{transaction_id}/track_with_guest_seller](https://docs.trustap.com/apis/openapi/online-shipping/basic.tracktransactionwithguestseller.md): Add tracking details for the guest seller specified
in the header as Trustap-User.

## Online Complaint

### Accept complaint for this online transaction

 - [POST /transactions/{transaction_id}/accept_complaint](https://docs.trustap.com/apis/openapi/online-complaint/basic.acceptcomplaint.md)

### Submit a detailed complaint for this online transaction

 - [POST /transactions/{transaction_id}/complain_with_description](https://docs.trustap.com/apis/openapi/online-complaint/basic.submitcomplaintwithdescription.md)

### Submit a detailed complaint for this online transaction for the guest buyer using Trustap-User

 - [POST /transactions/{transaction_id}/complain_with_guest_buyer](https://docs.trustap.com/apis/openapi/online-complaint/basic.submitcomplaintwithguestbuyer.md)

### End the complaint period for this online transaction

 - [POST /transactions/{transaction_id}/end_complaint_period](https://docs.trustap.com/apis/openapi/online-complaint/basic.endcomplaintperiod.md)

## Online Cancel

### Cancel this online transaction

 - [POST /transactions/{transaction_id}/cancel](https://docs.trustap.com/apis/openapi/online-cancel/basic.canceltransaction.md): Transaction can be cancelled until it's paid or if the feature
RequireSellerAcceptance is present, it can be cancelled
until it's tracked.
Offline access is allowed for this endpoint when the user has
granted the basic_tx:offline_cancel scope to the client
that is performing the request.

### Cancel this online transaction with a guest user

 - [POST /transactions/{transaction_id}/cancel_with_guest_user](https://docs.trustap.com/apis/openapi/online-cancel/basic.canceltransactionwithguestuser.md): This endpoint cancels a transaction for the user specified in the header
as Trustap-User.
Transaction can be cancelled until it's paid or if the feature
RequireSellerAcceptance is present, it can be cancelled until it's tracked.

## Online Buyer/Seller Details

### Get billing details from this online transaction

 - [GET /transactions/{transaction_id}/billing_details](https://docs.trustap.com/apis/openapi/online-buyerseller-details/basic.getbillingdetails.md)

### Get details of the buyer from this online transaction

 - [GET /transactions/{transaction_id}/buyer_details](https://docs.trustap.com/apis/openapi/online-buyerseller-details/basic.getbuyerdetails.md)

### Get details of the seller from this online transaction

 - [GET /transactions/{transaction_id}/seller_details](https://docs.trustap.com/apis/openapi/online-buyerseller-details/basic.getsellerdetails.md)

### Get the Stripe client secret for this online transaction

 - [GET /transactions/{transaction_id}/stripe_client_secret](https://docs.trustap.com/apis/openapi/online-buyerseller-details/getstripeclientsecretfortransaction.md): This endpoint is used for the asynchronous payment flow using
Stripe. The client_secret returned from this endpoint should
be passed to
stripe.confirmCardPayment
to start the payment process.

### Get the details for the Stripe publishable key that is in use for this online transaction

 - [GET /transactions/{transaction_id}/stripe_publishable_key](https://docs.trustap.com/apis/openapi/online-buyerseller-details/basic.getstripepublishablekeyfortransaction.md): Returns the Publishable Key for the Stripe Platform
which is hosting this transaction.

## F2F Transactions

F2F2 transactions are used when the goods are delivered in person.


### Get multiple face-to-face transactions by their IDs

 - [GET /p2p/batch/transactions](https://docs.trustap.com/apis/openapi/f2f-transactions/getp2ptransactionsbyids.md): This endpoint takes a required ids parameter that contains a
comma-separated list of transaction IDs, and returns the
transactions corresponding to those IDs. A maximum of 50 IDs may
be provided, and results are returned in the order specified in
ids, and duplicates are preserved. Transactions that couldn't
be found will be returned as null values in the result array.
The following example request results in the 200 OK response
described below, assuming that the transaction with ID 1990
couldn't be found:


 http
https://dev.stage.trustap.com/api/v1/p2p/batch/transactions?ids=1309,609,1990,609

### Get face-to-face transactions for the logged-in user

 - [GET /p2p/me/transactions](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.gettransactions.md): Transactions are currently sorted by ID.

### Create a new face-to-face transaction

 - [POST /p2p/me/transactions](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.createtransaction.md): Note that this endpoint is similar to POST /me/transactions
but takes deposit_price and deposit_charge fields instead of
the price and charge fields.
Offline access is allowed for this endpoint when the user has
granted the p2p_tx:offline_create_join scope to the client
that is performing the request.

### Create a new face-to-face transaction with both users

 - [POST /p2p/me/transactions/create_and_join](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.createandjointransaction.md): This endpoint is an optimised call that allows a transaction to
be created and joined in a single request. It requires the user
identified by join_user_id to have granted the
p2p_tx:offline_create_join scope to the client that is
performing the request.

Note that this endpoint is similar to POST /me/transactions
but takes deposit_price and deposit_charge fields instead of
the price and charge fields.

### Create a new face-to-face transaction with both users

 - [POST /p2p/me/transactions/create_with_guest_user](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.createtransactionwithguestuser.md): This endpoint is an optimised call that allows a transaction to
be created and joined in a single request. It requires the
online user to have granted the p2p_tx:offline_create_join scope
to the client that is performing the request.
This endpoint allows creating a transaction with both buyer and seller
as guest users, or one party to be guest user and the other full user.

### Get a face-to-face transaction by its ID

 - [GET /p2p/transactions/{transaction_id}](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.gettransaction.md)

### Claim a F2F transaction on behalf of the buyer

 - [POST /p2p/transactions/{transaction_id}/claim_for_buyer](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.claimforbuyer.md): This endpoint enables a client to claim a face-to-face transaction on behalf of a buyer.
This endpoint is only accessible for a client.

### Claim a F2F transaction on behalf of the seller

 - [POST /p2p/transactions/{transaction_id}/claim_for_seller](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.claimtransactionforseller.md): This endpoint enables a client to claim a face-to-face transaction on behalf of a seller.
This endpoint is only accessible for a client.

### Join the face-to-face transaction as guest user

 - [POST /p2p/transactions/{transaction_id}/join_with_guest](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.joinwithguest.md)

### Get metadata for a face-to-face transaction

 - [GET /p2p/transactions/{transaction_id}/metadata](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.gettransactionmetadata.md)

### Set metadata for a face-to-face transaction

 - [POST /p2p/transactions/{transaction_id}/metadata](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.settransactionmetadata.md)

### Claim F2F transaction as buyer given a claim secret

 - [POST /p2p/transactions_by_claim_secret/{secret}/claim_as_buyer](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.claimasbuyer.md): This endpoint allows a full user to claim a transaction
as buyer given a claim secret.

### Claim a F2F transaction as the seller

 - [POST /p2p/transactions_by_claim_secret/{secret}/claim_as_seller](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.claimtransactionasseller.md): This endpoint allows a full user to claim a transaction
as seller given a claim secret.

### Join a face-to-face transaction by its join code

 - [POST /p2p/transactions_by_join_code/{join_code}/join](https://docs.trustap.com/apis/openapi/f2f-transactions/p2p.jointransactionbyjoincode.md): This endpoint allows a user to join a transaction provided a join code. Offline access is allowed for this endpoint when the user has granted the p2p_tx:offline_create_join scope to the client that is performing the request.

### Get a face-to-face transaction by its join code

 - [GET /p2p/transactions_by_join_code/{joinCode}](https://docs.trustap.com/apis/openapi/f2f-transactions/getp2ptransactionbyjoincode.md)

## F2F Payment

Payment endpoints for face-to-face transactions.


### Get the Trustap fee for a face-to-face transaction

 - [GET /p2p/charge](https://docs.trustap.com/apis/openapi/f2f-payment/p2p.getcharge.md): This returns the Trustap fee, in the currency's smallest unit,
for a transaction involving goods with the supplied price. See
[the Stripe
documentation](https://stripe.com/docs/currencies#zero-decimal)
for more details.

### Update the description, currency, price and/or charge of this face-to-face transaction

 - [PATCH /p2p/transactions/{transaction_id}](https://docs.trustap.com/apis/openapi/f2f-payment/updatef2ftransaction.md)

### Accept the deposit for this face-to-face transaction

 - [POST /p2p/transactions/{transaction_id}/accept_deposit](https://docs.trustap.com/apis/openapi/f2f-payment/p2p.acceptdeposit.md): Offline access is allowed for this endpoint when the user has granted the p2p_tx:offline_accept_deposit scope to the client that is performing the request.

### Accept the deposit for this face-to-face transaction as a guest seller

 - [POST /p2p/transactions/{transaction_id}/accept_deposit_with_guest_seller](https://docs.trustap.com/apis/openapi/f2f-payment/p2p.acceptdepositwithguestseller.md)

### Get the bank transfer details for a face-to-face transaction which payment method is bank_transfer

 - [GET /p2p/transactions/{transaction_id}/bank_transfer_details](https://docs.trustap.com/apis/openapi/f2f-payment/p2p.getbanktransferdetails.md)

### Get the Stripe client secret for this face-to-face transaction

 - [GET /p2p/transactions/{transaction_id}/deposit_stripe_client_secret](https://docs.trustap.com/apis/openapi/f2f-payment/p2p.getdepositstripeclientsecretfortransaction.md): This endpoint is used for the asynchronous payment flow using
Stripe. The client_secret returned from this endpoint should
be passed to
stripe.confirmCardPayment
to start the payment process.

### Set deposit payment method for this face-to-face transaction

 - [POST /p2p/transactions/{transaction_id}/set_deposit_payment_method](https://docs.trustap.com/apis/openapi/f2f-payment/p2p.setdepositpaymentmethod.md): Set deposit payment method for face-to-face transaction. Values can be card,
bank_transfer, p24, and fpx.

### Set the total price for this face-to-face transaction

 - [POST /p2p/transactions/{transactionId}/set_price](https://docs.trustap.com/apis/openapi/f2f-payment/setpriceforp2ptransaction.md): price is the total price of the item, and thus includes the
price already paid for the deposit.

Note that this endpoint will return a remainder_too_low
response if the difference between the total price and the
deposit price is too low.
Offline access is allowed for this endpoint when the user has
granted the p2p_tx:offline_set_price scope to the client
that is performing the request.

### Skip the remainder for this face-to-face transaction

 - [POST /p2p/transactions/{transactionId}/skip_remainder](https://docs.trustap.com/apis/openapi/f2f-payment/skipremainderforp2ptransaction.md): This will skip the remainder payment of this transaction, and
the total price of the transaction will not need to be set. Note
that this must be done before the total price of the transaction
is set using /set_price.

## F2F Handover

Handover endpoints for face-to-face transactions.


### Confirm the handover for this face-to-face transaction

 - [POST /p2p/transactions/{transaction_id}/confirm_handover](https://docs.trustap.com/apis/openapi/f2f-handover/p2p.confirmhandover.md): Both parties should confirm the handover for a face-to-face
transaction. The complaints period begins when the seller
confirms the handover.
Offline access is allowed for this endpoint when the user has
granted the p2p_tx:offline_confirm_handover scope to the client
that is performing the request.

### Confirm the handover for this face-to-face transaction with a guest user

 - [POST /p2p/transactions/{transaction_id}/confirm_handover_with_guest_user](https://docs.trustap.com/apis/openapi/f2f-handover/p2p.confirmhandoverwithguestuserfortransaction.md): Both parties should confirm the handover for a face-to-face
transaction. Full users don't have access to this endpoint.

## F2F Complaint

Complaint endpoints for face-to-face transactions.


### Accept a complaint for this face-to-face transaction

 - [POST /p2p/transactions/{transaction_id}/accept_complaint](https://docs.trustap.com/apis/openapi/f2f-complaint/p2p.acceptcomplaint.md)

### Submit a complaint for this face-to-face transaction

 - [POST /p2p/transactions/{transaction_id}/complain](https://docs.trustap.com/apis/openapi/f2f-complaint/p2p.submitcomplaint.md): Offline access is allowed for this endpoint when the user has granted the p2p_tx:offline_complain scope to the client that is performing the request.

### Submit a complaint for this face-to-face transaction with a guest buyer

 - [POST /p2p/transactions/{transaction_id}/complain_with_guest_buyer](https://docs.trustap.com/apis/openapi/f2f-complaint/p2p.submitcomplaintwithguestbuyer.md)

### End the complaint period for this face-to-face transaction with a guest buyer

 - [POST /p2p/transactions/{transaction_id}/end_complaint_period_with_guest_buyer](https://docs.trustap.com/apis/openapi/f2f-complaint/p2p.endcomplaintperiodwithguestbuyer.md)

## F2F Cancel

Cancel endpoints for face-to-face transactions.


### Cancel this face-to-face transaction with a description

 - [POST /p2p/transactions/{transaction_id}/cancel_with_description](https://docs.trustap.com/apis/openapi/f2f-cancel/p2p.cancelwithdescription.md): If a transaction is cancelled after the deposit has been paid
then the deposit (excluding the Trustap fee) will be returned to
the buyer.
Offline access is allowed for this endpoint when the user has
granted the p2p_tx:offline_cancel scope to the client
that is performing the request.

### Cancel this face-to-face transaction with a description with guest user

 - [POST /p2p/transactions/{transaction_id}/cancel_with_description_with_guest_user](https://docs.trustap.com/apis/openapi/f2f-cancel/p2p.cancelp2ptransactionwithdescriptionasguest.md): If a transaction is cancelled after the deposit has been paid
then the deposit (excluding the Trustap fee) will be returned to
the buyer.

## F2F Buyer/Seller Details

User details endpoints for face-to-face transactions.


### Get billing details from this p2p transaction

 - [GET /p2p/transactions/{transaction_id}/billing_details](https://docs.trustap.com/apis/openapi/f2f-buyerseller-details/p2p.getbillingdetails.md)

### Get the Stripe client secret for this face-to-face transaction

 - [GET /p2p/transactions/{transaction_id}/remainder_stripe_client_secret](https://docs.trustap.com/apis/openapi/f2f-buyerseller-details/p2p.getremainderstripeclientsecretfortransaction.md): This endpoint is used for the asynchronous payment flow using
Stripe. The client_secret returned from this endpoint should
be passed to
stripe.confirmCardPayment
to start the payment process.

### Get the details for the Stripe publishable key that is in use for this face-to-face transaction

 - [GET /p2p/transactions/{transaction_id}/stripe_publishable_key](https://docs.trustap.com/apis/openapi/f2f-buyerseller-details/p2p.getstripepublishablekeyfortransaction.md): Returns the Publishable Key for the Stripe Platform
which is hosting this transaction.

### Get the details of the buyer from this face-to-face transaction

 - [GET /p2p/transactions/{transactionId}/buyer_details](https://docs.trustap.com/apis/openapi/f2f-buyerseller-details/getbuyerdetailsfromp2ptransaction.md)

### Get the details of the seller from this face-to-face transaction

 - [GET /p2p/transactions/{transactionId}/seller_details](https://docs.trustap.com/apis/openapi/f2f-buyerseller-details/getsellerdetailsfromp2ptransaction.md)

## Trustap Lite

### Creates a transaction with guest seller and returns actions
page url

 - [POST /p2p/listings/create_with_seller](https://docs.trustap.com/apis/openapi/trustap-lite/p2p.createwithseller.md)

## Notifications

Notifications log updates for users.


### Get the notifications for the logged-in user

 - [GET /me/notifications](https://docs.trustap.com/apis/openapi/notifications/app_notifs.getnotifications.md)

### Get a notification by its ID

 - [GET /notifications/{notification_id}](https://docs.trustap.com/apis/openapi/notifications/app_notifs.getnotification.md)

### Mark this notification as read

 - [POST /notifications/{notification_id}/mark_as_read](https://docs.trustap.com/apis/openapi/notifications/app_notifs.marknotificationasread.md)
