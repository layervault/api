# LayerVault API Reference

## Introduction
The LayerVault API provides a simple HTTP based REST based mechanism for interacting with LayerVault and allowing applications to access user information via oAuth 2 authenticated requests, over SSL. API responses are simple JSON objects.

> ## API Endpoint
> https://layervault.com/api/v1
> ### Summary of Resource URL Patters
>     /v1/me
>     /v1/keypair(.:format)
>     /v1/:organization_name
>     /v1/:organization_name/:project
>     /v1/:organization_name/:project/move
>     /v1/:organization_name/:project/color
>     /v1/:organization_name/:project(/*path)
>     /v1/:organization_name/:project(/*path)/move
>     /v1/:organization_name/:project(/*path)/color
>     /v1/:organization_name/:project(/*path)/:file_name
>     /v1/:organization_name/:project(/*path)/:file_name/move
>     /v1/:organization_name/:project(/*path)/:file_name/sync_check
>     /v1/:organization_name/:project(/*path)/:file_name/:revision
>     /v1/:organization_name/:project(/*path)/:file_name/:revision/previews
>     /v1/:organization_name/:project(/*path)/:file_name/:revision/revisions
>     /v1/:organization_name/:project(/*path)/:file_name/:revision/meta

## Getting Started

0. Register an API client from your Account Settings under the Applications menu.
0. For the Redirect URI, use ```urn:ietf:wg:oauth:2.0:oob``` for now. Later on, if you were building a Web application for an API client, you would specify your post-user authorization redirect URL here.
0. Note your ```client_id``` and ```client_secret```.
0. Try the example Access Token request below.


## Authentication and Requesting Access Tokens

All API requests are required to be authenticated via an oAuth 2 access token. It's easy to request a token via the command line to get a feel for how things work:

    curl -i https://layervault.com/oauth/token \
    -F grant_type="password" \
    -F username="<your layervault username>" \
    -F password="<your layervault password>" \
    -F client_id="<client_id_goes_here>" \
    -F client_secret="<client_secret_goes_here>"


Which will return an Access token response like this one:

    {
      "access_token":"aec9c670cf5e673bfedf83d055d2a2e0e5f37e52d3b41cffcf7874f73a7458bf",
      "token_type":"bearer",
      "expires_in":7200,
      "refresh_token":"afe9c670cf5e673bfedf83d055d2a2e0e5f37e52d3b41cffcf7874f73a7458bf",
      "scope":"user"
    }

### Configuring A Web Application as a LayerVault API client

First of all, make sure that your redirect URI is correctly specified under your Application settings under your Layervault Account settings.

Then, when configuring your oAuth 2 client library, tell it to use ```https://layervault.com/oauth/authorize``` to request authorization and ```https://layervault.com/oauth/token``` to get access tokens. Most libraries will default to this convention anyway.

If you're building a Rails application and using OmniAuth, we have a pre-baked Authentication Strategy for you [here](https://github.com/layervault/omniauth-layervault).

### Refresh Tokens

The LayerVault API implements Refresh Tokens, allowing you to request a new access token using the refresh token, thus saving the user from having to re-authorize your application. The Refresh token is provided when the Access token is requested, along with the expiration time of the Access token, before which your refresh token should be used.

## Making API Calls

Once you have your access token, making some API calls from the command line is also easy:

```
curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/me'
```

## Versioning

Currently, the LayerVault API is versioned with a URL segment, such as ```/v1/```. The current production version of the API is Version 1, so your calls should use ```/v1/``` as the version segment.

## Rate Limits

The API currently limits the requests you can make against it hourly. We have provided two response headers with each request to the API with Rate Limiting Information. They are returned from every API call.

    X-RateLimit-Limit: 300
    X-RateLimit-Remaining: 299

`X-RateLimit-Limit` is your current limit per hour. `X-RateLimit-Remaining` is how many requests you have left. If you need more requests than 300/hr, please [contact us](mailto:support@layervault.com) and we'll do our best to accommodate you.

## Client Libraries

[The Official Ruby LayerVault Client](https://github.com/layervault/layervault_ruby_client)

# Methods

## Key Pairs

### Retrieving Key Pairs

This call returns a set of KeyPairs for future uploading of files. .

>  Definition
>
>     GET /api/v1/keypair
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/keypair'
>
>  Example Response
>
>     {"access_key_id":"AKIAIMLDM23CBAOOEXNQ","secret_access_key":"/fPHLA5505nSp0P0c2gsbYd19HoEBZHpRoaj4U3G"}

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```access_key_id``` - The Access key ID
  - ```secret_access_key``` - The Secret Access Key

## User Information

### Retrieving Organization Information

This call returns the user information for which the Client is acting on behalf of.

>  Definition
>
>     GET /api/v1/me
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/me'
>
>  Example Response
>
>     {"email":"john@layervault.com","first_name":"John","last_name":"McDowall","is_admin":true}

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```email``` - The email address of the user.
  - ```first_name``` - The First name of the user.
  - ```last_name``` - The Last name of the user.
  - ```is_admin``` - Whether the user is an admin of the site or not.

## Organization

### Retrieving Organization Information

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

## Projects

### Retrieving Project Information

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Creating a Project

This call returns the organization information for the requested Organization.

>  Definition
>
>     POST /api/v1/:organization_name/:project
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Deleting a Project

This call returns the organization information for the requested Organization.

>  Definition
>
>     DELETE /api/v1/:organization_name/:project
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Moving a Project

This call returns the organization information for the requested Organization.

>  Definition
>
>     POST /api/v1/:organization_name/:project/move
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Changing a Project's Folder color

This call returns the organization information for the requested Organization.

>  Definition
>
>     PUT /api/v1/:organization_name/:project
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

## Folders

### Retrieving Folders's Information

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project/:folder_name
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Creating a Folder

This call returns the organization information for the requested Organization.

>  Definition
>
>     POST /api/v1/:organization_name/:project/:folder_name
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Deleting a Folder

This call returns the organization information for the requested Organization.

>  Definition
>
>     DELETE /api/v1/:organization_name/:project/:folder
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Moving a Folder

This call returns the organization information for the requested Organization.

>  Definition
>
>     POST /api/v1/:organization_name/:project/:folder/move
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Changing a Folder's color

This call returns the organization information for the requested Organization.

>  Definition
>
>     PUT /api/v1/:organization_name/:project/:folder
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

## Files

### Retrieving Files's Information

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project/:folder/:file_name
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Creating a File

This call returns the organization information for the requested Organization.

>  Definition
>
>     PUT /api/v1/:organization_name/:project/:folder/:file_name
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Deleting a File

This call returns the organization information for the requested Organization.

>  Definition
>
>     DELETE /api/v1/:organization_name/:project/:folder/:file_name
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Moving a File

This call returns the organization information for the requested Organization.

>  Definition
>
>     POST /api/v1/:organization_name/:project/:folder/:file_name
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Performing a Sync check on a File

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project/:folder/:file_name/sync_check
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

## Revisions

### Retrieving Revisions' Information

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project/:folder/:file_name/:revision
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Retrieving Revision's Revision History

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/revisions
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Retrieving Revision's Meta Information

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/meta
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Retrieving Revision's Previews Information

This call returns the organization information for the requested Organization.

>  Definition
>
>     GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/previews
>
>  Example Request
>
>     $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'
>
>  Example Response
>
>     {
>       "name":"LayerVault",
>       "deleted_at":"3013-02-26T16:53:38Z",
>       "updated_at":"2013-10-22T00:11:57Z",
>       "full_url":"https://layervault.com/layervault",
>       "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
>     }

### Arguments
No arguments are required.

### Returns

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.
