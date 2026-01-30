# Authentication

The Trustap API uses authentication to ensure that only authorized
clients can access and interact with resources. This guide explains the
types of authentication supported, how to use them, and when to use each
method.

Trustap uses authentication to do the following.

* Identify who is making a request.
* Prevent unauthorized access to user or transaction data.


## Types of Authentication Supported

Trustap supports multiple ways to authenticate API requests. We recommend Basic Authentication for most integrations. If you need to perform actions on behalf of a specific user, include the `Trustap-User` header. OAuth 2.0 is also available, but generally not required for partner integrations

| Auth type | Description | Use cases |
|  --- | --- | --- |
| Basic Authentication (API key) | Use your Trustap API key (Base64 encoded) in the `Authorization: Basic header`. | - Recommended method for partner integrations.
- Use for administrative tasks including user and transaction management.

 |
| Basic Authentication + `Trustap-User` header | Combine Basic Auth with a `Trustap-User: user ID` header to act on behalf of a specific user. | - Use for tasks on behalf of a user including tasks like payment management, delivery/handover confirmation,  transaction cancelling, and complaint submission.

 |
| Bearer Token (OAuth 2.0) | Use an OAuth 2.0 access token in the `Authorization: Bearer` header. Tokens are obtained via the [OAuth flow](/docs/guides/transactions/full-users). | - Available but not recommended method for authentication.
- Use for advanced third-party integrations needing fine-grained, per-user access control.

 |


## Authentication examples

### Basic Authentication

In cURL you can use `--user` and use the key you received during [setup](/docs/intro/auth). Alternatively, you can use the header object `--header "Authorization: Basic $(echo -n '<YOUR_TOKEN_HERE>:' | base64)"`.


```CURL Basic Authentication - Create a guest buyer
curl --location 'https://dev.stage.trustap.com/api/v1/guest_users' \
--header 'Content-Type: application/json' \
--user '<API_KEY>:' \
--data-raw '{"email":"bert.gray@my-mail.com","first_name":"Bert","last_name":"Gray","country_code":"at","tos_acceptance":{"unix_timestamp":1736354931,"ip":"127.0.0.1"}}'
```

### Basic Authentication with `Trustap-User`


```CURL Basic Authentication with Trustap-User - Confirm delivery
curl -i -X POST \
  'https://dev.stage.trustap.com/api/v1/transactions/{transaction_id}/confirm_delivery_with_guest_buyer' \
  --user '<API_KEY>:' \
  --header 'Trustap-User: <USER_ID>'
```

### Authentication using Bearer token


```CURL Authentication with Bearer token - Confirm delivery
curl -i -X POST \
  'https://dev.stage.trustap.com/api/v1/transactions/{transaction_id}/confirm_delivery_with_guest_buyer' \
  --header 'Authorization: Bearer <BEARER_API_TOKEN>' \
```