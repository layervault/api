## Authentication and Requesting Access Tokens

All API requests are required to be authenticated via an oAuth 2 access token. It's easy to request a token via the command line to get a feel for how things work:

```shell
curl -i https://api.layervault.com/oauth/token \
  -F grant_type="password" \
  -F username="<your layervault username>" \
  -F password="<your layervault password>" \
  -F client_id="<client_id_goes_here>" \
  -F client_secret="<client_secret_goes_here>"
```

Which will return an Access token response like this one:

```json
{
  "access_token":"aec9c670cf5e673bfedf83d055d2a2e0e5f37e52d3b41cffcf7874f73a7458bf",
  "token_type":"bearer",
  "expires_in":7200,
  "refresh_token":"afe9c670cf5e673bfedf83d055d2a2e0e5f37e52d3b41cffcf7874f73a7458bf",
  "scope":"user"
}
```
