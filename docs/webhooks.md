# Webhooks

Webhooks are HTTP messages that one application automatically sends to another when specific events occur. The Trustap API sends webhooks for events such as receiving a payment for a transaction or delivering a parcel.
Use webhooks in your application to learn about the status of a transaction and start your own processes without polling the Trustap API.

Along with webhooks, the Trustap API also sends emails to users during the life of the transaction. Review the [emails guide](/docs/concepts/emails) for more information.

## Set up your webhook

To receive webhook events from Trustap, create a webhook handler for your application. Webhook handlers listen for and process incoming HTTP requests sent by a third-party service including a webhook event.

To set up your Trustap webhooks, share your `<WEBHOOK_ENDPOINT>` URL (for
example, `https://your-app.com/webhooks`) with Trustap as part of your
[setup](/docs/intro/auth). Trustap sends all webhook events associated
with your account to this URL as `POST` requests, so your firewall rules and
endpoint handlers should be set up to handle these.

Please note that, if multiple clients are being set up, then each should use a
distinct webhook endpoint, especially across environments. A simple way of
achieving this is creating a unique endpoint path for each client.

## Webhook event codes

The Trustap API supports the following event codes.

### Event codes for online transactions

* `basic_tx.joined`
* `basic_tx.rejected`
* `basic_tx.cancelled`
* `basic_tx.claimed`
* `basic_tx.listing_transaction_accepted`
* `basic_tx.listing_transaction_rejected`
* `basic_tx.payment_failed`
* `basic_tx.paid`
* `basic_tx.payment_refunded`
* `basic_tx.payment_review_flagged`
* `basic_tx.payment_review_finished`
* `basic_tx.tracking_details_submission_deadline_extended`
* `basic_tx.tracked`
* `basic_tx.delivered`
* `basic_tx.complained`
* `basic_tx.complaint_period_ended`
* `basic_tx.funds_released`
* `basic_tx.funds_refunded`


### Event codes for face-to-face transactions

* `p2p_tx.joined`
* `p2p_tx.rejected`
* `p2p_tx.cancelled`
* `p2p_tx.claimed`
* `p2p_tx.deposit_payment_failed`
* `p2p_tx.deposit_paid`
* `p2p_tx.deposit_review_flagged`
* `p2p_tx.deposit_review_finished`
* `p2p_tx.deposit_refunded`
* `p2p_tx.deposit_accepted`
* `p2p_tx.priced`
* `p2p_tx.remainder_skipped`
* `p2p_tx.remainder_paid`
* `p2p_tx.remainder_review_flagged`
* `p2p_tx.remainder_review_finished`
* `p2p_tx.buyer_handover_confirmed`
* `p2p_tx.seller_handover_confirmed`
* `p2p_tx.complained`
* `p2p_tx.funds_released`
* `p2p_tx.funds_refunded`


## Payload

The following is an example payload the Trustap API could pass to a webhook endpoint.


```json
{
    "code": "p2p_tx.joined",
    "user_id": "feb33a87-3917-4538-9260-127c8a6b5232",
    "target_id": "1309",
    "target_preview": {
        "id": 1309,
        "status": "joined",
        "currency": "eur",
        "deposit_pricing": {
            "price": 1234,
            "charge": 78
        },
        "description": "Soccer ticket",
        "skip_remainder": false,
        "created": "2019-12-25T09:00:00.000Z",
        "seller_id": "ad5bb99f-85bf-47e1-be0d-15e7541c6ad7",
        "buyer_id": "feb33a87-3917-4538-9260-127c8a6b5232",
        "joined": "2019-12-25T10:00:00.000Z"
    },
    "time": "2025-12-25T10:00:00.000Z"
}
```

The following fields may be included in a webhook payload.

| Field  | Type | Description |
|  --- | --- | --- |
| `code` | string | A code that identifies the type of event that occurred to the target resource. |
| `user_id` | string | The ID of the user who triggered the event, if applicable. For example, if the buyer submits a payment, this field contains the buyer's user ID. If present on a transaction event, the user ID must match either the `seller_id` or the `buyer_id` of the transaction. This field is omitted if the event was triggered by an automated system, such as when funds are released after the complaint period ends. |
| `target_id` | string | Returned as a string for consistency, even though transaction resources use integer IDs. In such cases, the target_id is a string representation of the integer. It is safe to convert between the two. |
| `target_preview` | object | An optional object that previews the target resource. If present, it allows you to avoid making an additional API request to fetch the full resource. |
| `time` | string | The time at which the event occurred. |


## Authentication

Trustap includes a Basic Authentication header in each webhook request. You can use the username and password to verify the request is from Trustap. Set these credentials when configuring your webhook endpoint and as part of your [Trustap setup](/docs/intro/auth).

## Testing

During testing, before you have created your own public webhook handler, it can be beneficial to use [online webhook testing sites](https://webhook.site/). Work with the Trustap team to temporarily configure your Trustap test environment with a <WEBHOOK_ENDPOINT> from an online webhook testing site.

While you can test your webhooks using public services, it's important to understand that you are sharing your information with these third-parties. Only use test data. Never share production data. The mention of a specific company or product name is solely for educational purposes and does not imply endorsement by Trustap.

To test your webhooks, you can trigger a webhook by creating a transaction. Complete the following steps to trigger a webhook for `basic_tx.joined` code.

1. [Create a guest seller](/docs/guides/transactions/online/online-int-path-cc#step-1-create-a-guest-seller)
2. [Create a guest buyer](/docs/guides/transactions/online/online-int-path-cc#step-2-create-a-guest-buyer)
3. [Calculate the charge](/docs/guides/transactions/online/online-int-path-cc#step-3-get-trustap-fee-charge)
4. [Create a transaction](/docs/guides/transactions/online/online-int-path-cc#step-4-create-a-transaction-with-a-guest-seller-and-guest-buyer)


### Expected payload

If your webhook handler is working correctly, you will receive a webhook with the following payload.


```JSON
{
  "code": "basic_tx.joined",
  "user_id": null,
  "target_id": "25992",
  "target_preview": {
    "id": 25992,
    "status": "joined",
    "currency": "gbp",
    "quantity": 1,
    "price": 20000,
    "charge": 640,
    "charge_seller": 0,
    "description": "Online Transaction",
    "created": "2025-04-09T10:08:42Z",
    "is_payment_in_progress": false,
    "client_id": "5a3d7990-9f1c-4e11-8d67-3da5b160c50a",
    "buyer_id": "1-8f48bf31-a291-4bc8-8fc8-9e4eb482fe12",
    "seller_id": "1-c6381146-9374-412d-9a44-4ee4386eb69a",
    "joined": "2025-04-09T10:08:42Z"
  },
  "time": "2025-04-09T10:08:41.905612674Z",
  "metadata": {}
}
```

### Simulate Trustap webhook events

To test your webhook listener, use the following tool to create a sample webhooks event.

1. Enter your `Webhook listener URL`, optional auth, and select your `Sample payload`.
2. Copy, paste, and run the generated cURL command in your local terminal to send the sample payload to your webhook listener.