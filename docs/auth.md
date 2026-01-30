# Get API keys

To get started using the Trustap API, complete these steps.

### Contact Trustap

Use our [contact form](https://www.trustap.com/contact-us/) get in touch with us.

To help us understand your use case, please provide the following.


```
* What does your company do?
* What problem can Trustap solve for you?
```

Our integration specialist will contact you as soon as possible.

### Send your configuration details

To support your Trustap integration, our integration specialist will ask for the following details.

| Integrator supplied URL | Description |
|  --- | --- |
| `<WEBHOOK_ENDPOINT>` | The URL where Trustap will send webhook events for your account. |
| `<REDIRECT_URI>` | The callback URL used after [full Trustap account creation](/docs/guides/transactions/full-users). This must match the redirect URI used in your flow. Trustap supports wildcard URIs. For example, if you provide `https://www.sams-bikes.com/*`, you can redirect to `https://www.sams-bikes.com/login_path/`. |
| `<CALLBACK_PAYMENT>` | The URL to which users are redirected after [completing payment](/docs/guides/transactions/online/online-int-path-cc#step-5-buyer-transfers-funds). Trustap also supports wildcards here. For example, `https://www.sams-bikes.com/payment/*`. |


### Use your authentication credentials

Your integration specialist will provide the credentials needed to authenticate with the Trustap API.

* API key: `<API_KEY>`
* Client ID: `<CLIENT_ID>`
* Client secret: `<CLIENT_SECRET>`
* Webhook username: `<WEBHOOK_USER>`
* Webhook password: `<WEBHOOK_PASS>`


To make an authenticated API request, use your API key as the username in HTTP Basic Auth. Leave the password field blank.


```CURL
curl --location 'https://dev.stage.trustap.com/api/v1/guest_users' \
--header 'Content-Type: application/json' \
--user '<API_KEY>:' \
--data-raw '{"email":"sue.glass@my-mail.com","first_name":"Sue","last_name":"Glass","country_code":"ca","tos_acceptance":{"unix_timestamp":1736441841,"ip":"127.0.0.1"}}'
```

For basic auth authentication, API keys are provided as the "username" portion of `<username:password>`. The password field is blank. Remember to include colon (`:`) as part of API key.  For example `--user '7625348723743435:'`.