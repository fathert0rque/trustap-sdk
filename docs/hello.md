# Hello Trustap

Use this guide to make your first Trustap API call.

## Make your first call


```CURL Create a guest buyer
curl --location 'https://dev.stage.trustap.com/api/v1/guest_users' \
--header 'Content-Type: application/json' \
--user 'd74b258f-2978-4602-a04c-846e44bd8908:' \
--data-raw '{"email":"bert.gray@my-mail.com","first_name":"Bert","last_name":"Gray","country_code":"at","tos_acceptance":{"unix_timestamp":1736354931,"ip":"127.0.0.1"}}'
```

## Next steps

1. Follow our [setup](/docs/intro/auth) guide to get your API key and start developing for your own account.
2. Start working with our [guides](/docs/guides/transactions/face-to-face/face-to-face-intro).
3. Explore our [API reference](/apis/openapi).