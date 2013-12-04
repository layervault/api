### Configuring A Web Application as a LayerVault API client

First of all, make sure that your redirect URI is correctly specified under your Application settings under your Layervault Account settings.

Then, when configuring your oAuth 2 client library, tell it to use ```https://api.layervault.com/oauth/authorize``` to request authorization and ```https://api.layervault.com/oauth/token``` to get access tokens. Most libraries will default to this convention anyway.

If you're building a Rails application and using OmniAuth, we have a pre-baked Authentication Strategy for you [here](https://github.com/layervault/omniauth-layervault).
