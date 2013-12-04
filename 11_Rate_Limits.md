## Rate Limits

The API currently limits the requests you can make against it hourly. We have provided two response headers with each request to the API with Rate Limiting Information. They are returned from every API call.

    X-RateLimit-Limit: 300
    X-RateLimit-Remaining: 299

`X-RateLimit-Limit` is your current limit per hour. `X-RateLimit-Remaining` is how many requests you have left. If you need more requests than 300/hr, please [contact us](mailto:support@layervault.com) and we'll do our best to accommodate you.
