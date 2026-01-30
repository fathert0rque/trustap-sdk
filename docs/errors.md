# Errors and status codes

## API Errors

If a request contains an invalid value (based on the `validation` property of a given field), the API returns a 400 `Bad Request` error detailing the first invalid field found. If one field fails validation, none of the provided fields will be updated.

An incorrect value that is otherwise valid (such as a phone number that is formatted correctly, but which
isn't registered) will not result in an error. Instead, these values are stored, and the field's status is updated later to reflect the verification result.

For the best user experience, mirror the API's validation checks on the frontend and only submit values that pass these checks.

## API status codes

The following status codes are used by the Trustap API when reporting success or failure of API requests.

| Code | Description |
|  --- | --- |
| `200`, `201` | OK. Everything worked as expected. |
| `400` | Bad Request. The request was unacceptable, often due to a missing required parameter or invalid value |
| `401` | Unauthorized. No valid API key provided. Check your API Key or follow our [setup](/docs/intro/auth) guide if you do not yet have one. |
| `403` | Forbidden. The API key does not have permissions to perform the request. If you are marking a call on behalf of a user, ensure your header includes `Trustap-User` with the ID of your user. |
| `404` | Not Found. The requested resource does not exist. |
| `409` | Conflict. Contact Trustap support for assistance. |
| `500` | Server Errors. Something went wrong on Trustap’s end. |