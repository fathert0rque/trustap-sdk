# Pricing

Trustap uses a payment model where end-users pay a small fee to add Trustap's
protection to their transactions. Trustap does not charge third-party clients to
integrate Trustap into their services.

This section details how the Trustap fee should be retrieved and handled when
using the TrustAPI.

## Handling Trustap Pricing

With the TrustAPI, Trustap's fees are calculated when a transaction begins,
before payment has been made. This allows users to know ahead of time how much
it will cost to use Trustap for a particular transaction.

In general, handling the pricing of a Trustap transaction consists of 3 steps:

1. Retrieve Trustap's fees using a plain get request (note that this will
require an active access token). For example:



```bash
    curl \
        -H "Authorization: Bearer $access_token" \
        https://dev.stage.trustap.com/api/v1/charge?currency=usd&price=12300
```

1. Show the fees to the user.
2. Create a new transaction (or update an existing transaction) by adding the
fields provided by the call to `/api/v1/charge` to a transaction
creation/update request.


## Model Endpoints

The online model and the face-to-face model use different endpoints for
calculating fees:

* Online: [`GET /api/v1/charge`](https://api.docs.trustap.com/#operation--charge-get)
* Face-to-face: [`GET /api/v1/p2p/charge`](https://api.docs.trustap.com/#operation--p2p-charge-get)


## Charge Calculator Version

The `/charge` endpoints return an additional `charge_calculation_version` field.
This field is useful for the rare occasions where Trustap changes its pricing
calculation, in which case the client can be notified that such a change has
occurred, instead of the request simply failing due to a discrepancy in the
`charge` value.

## Charge display

The charge a buyer pays for a transaction is shown as `Buyer Protection Fee` in the Trustap payment screen. This term can be customised through a request from your Trustap integration specialist.

Trustap payment screen showing Buyer Protection free
## Charge configuration

See our [charge configuration guide](/docs/guides/charge-config) for more information on to setup and use different charge configurations with the Trustap API.

## Platform commission

Trustap allows platforms to collect additional commission on each transaction. This commission can help cover operational costs or other platform-specific fees.

* Commission can be configured to be charged to the seller, buyer, or both.
* Commission can be charged as either a percentage of the transaction or as a flat fee.
* Seller fees are not displayed in the Trustap payment screen but are available in the [charge endpoint](/apis/openapi/online-payment/basic.getcharge#online-payment/basic%2Egetcharge/t=response&c=200&path=charge_seller).
* Collected commissions are held in the Trustap vault and manually paid out at agreed intervals.
* To enable and configure platform commission and other fees, contact your Trustap integration specialist.