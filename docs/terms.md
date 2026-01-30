# Terms

## Transaction

In Trustap, a transaction is a secure process designed to facilitate safe buying and selling between two parties.
Transactions always have two parties.

## Face-to-Face transaction

A Face-to-face (F2F) transaction is a transaction in where an item, digital goods or service could be delivered in person or transferred through a client's platform (like a digital download).
In these transactions, delivery of the item is confirmed by the user on the client's platform or automatically by the client.

## Online transaction

An online transaction is a transaction where an item requires delivery using a courier service.
In these transactions, delivery of the item is confirmed automatically by integrating to a 3rd party which informs Trustap of the item's delivery status.

## Transaction Flow

Every transaction has a lifecycle where it moves from one state to another. Depending on the state, users can perform different actions. Usually, there is one action which moves the transaction further and one action for cancelling the transaction. If the transaction is cancelled and there is payment, the buyer gets refunded automatically (usually within 1 hour).

## Users

Trustap supports two types of users, full and guest. Full users have a Trustap Account and every seller will have to become a full user to be able to receive payout. Guest users don’t have a Trustap Account, but they can pay for the transaction and even lodge a complaint through a clients’s platform. If they would like to get more information and have information about their transaction, they can create a full user account.

Every transaction consists of two parties, one seller and one buyer. To create a transaction and redirect buyers to pay for it on the Trustap payment page, both parties will have to be created. After they are created, their Trustap IDs should be reused for every future transaction.

## Trustap-User

Trustap is a user-oriented platform. This means that every action is completed by a user or on behalf of a consenting user.
To make an API call on behalf of another Trustap user, you must do the following.

1. Create a user with the appropriate scopes.
  * [Create face-to-face user](/docs/guides/transactions/full-users).
  * [Create online user](/docs/guides/transactions/full-users).
2. Add `Trustap-User` to the header of your API call with your user's `id`.



```CURL Example of Trustap-User being used when making a complaint
curl --location 'https://dev.stage.trustap.com/api/v1/p2p/transactions/{transaction_id}/complain_with_guest_buyer' \
--header 'Content-Type: application/json' \
--header 'Trustap-User: 1-615a70d4-b624-4243-95be-a8367b7953ea' \
--user '<API_KEY>:' \
--data '{
"description": "Item was fake"
}'
```

## Payments

To process payments, Trustap is partnered with Stripe.

### Card Payments

To pay for the transaction the buyer needs to be redirected to a Trustap payment page which contains all information about the transaction and the fee that is charged to the buyer. Multiple URLs are used to pay for the transaction. Depending on the type of transaction and type of the user, the following URLs should be used:

| **URL** `https://actions.<realm>.trustap.com` | **Guest buyer** | **Full buyer** |
|  --- | --- | --- |
| Online transaction | /online/transactions/:id/guest_pay | /online/transactions/:id/pay |


Important realms that are available for integrations are:

* `<realm>` = “” → production realm
* `<realm>` = “trustap-stage” → testing realm


### Bank Transfer

To pay for the transaction the buyer needs to be redirected to a page which contains all Trustap banking details to make the bank transfer, along with information about the transaction and the fee that is charged to the buyer. In this case, this page will be partner-generated or default one from Stripe. Important information or Stripe URL will be returned in a special API call.

## Claiming

Claiming is the process where a full Trustap user claims the transaction which was created with a guest user. This can happen in two ways:

* programmatically via API
* through email and Trustap app


### Programmatic claim

Programmatically via API is the process where a partner in the integration needs to include calling specific endpoints with the correct Trustap User ID in the header. This part is specifically sensitive from the security perspective because it gives a partner functionality/possibility to claim transactions for a “wrong” user.

### Email claim

Claiming through email and the Trustap app is implemented in a way that a user gets email on an email address they used on a partner’s side. In the email, there is a claiming link which redirects the user to the Trustap application where they, after registering, claim a transaction.

## Emails

Trustap sends the following groups of emails:

* status of transactions
* reminders to complete account profile
* instructions on how to claim transaction (if the transaction was created with a guest user)


Every transaction can go through multiple statuses which can lead to many emails being received by users. This can be also true for reminders if the user decides to abandon the transaction. If a partner decides, Trustap can disable or enable all emails. You cannot choose which emails are sent.

## Webhooks

Trustap sends webhook calls to configured webhook endpoint using basic authentication (username + password). Every request contains information about a [resource](/docs/concepts/webhooks) and the code which triggered this webhook call. Usually, one webhook handler is enough, but could be configured to use multiple as well. This is useful when testing on a Stage environment.
It’s advisable to update the internal representation of a transaction whenever a webhook call is received.

## Payouts

To be able to receive payouts, sellers will need to make a full account on Trustap and fill in their personal details and desired payout method. After completing these details, Trustap may ask them to upload an identity document to verify their identity. Verification is done by Trustap’s partner Stripe.
It’s possible that some data isn’t required at the moment of registration, but it becomes required later. For example, once the seller has made a certain volume of payouts they may need to verify their identity.
This information can be found through the [payout status endpoint](/apis/openapi/personal/getprofilepayoutstatus).

## Escrow

An escrow-style platform is a payment service in which a neutral third party temporarily holds funds during a transaction and releases them only once all agreed conditions have been met by both the buyer and the seller. For example, payment is held by a third party and released to the seller after the buyer confirms delivery.
See our [Merchant of Record](/docs/concepts/merchant-of-record) guide for more information.