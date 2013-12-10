### Refresh Tokens

The LayerVault API implements Refresh Tokens, allowing you to request a new access token using the refresh token, thus saving the user from having to re-authorize your application. The Refresh token is provided when the Access token is requested, along with the expiration time of the Access token, before which your refresh token should be used.

#### Manually applying the Refresh Token

If you are not using an oAuth 2 library to handle your tokens (recommended), it is possible to apply the refresh token and obtain a new access token by making a new ```GET``` request with the following parameters

```shell
curl -i http://localhost:3000/oauth/token \
   -F grant_type="refresh_token" \
   -F refresh_token="<refresh_token>" \
   -F client_id="<client_id>" \
   -F client_secret="<client_secret>"
```
