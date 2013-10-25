# LayerVault API Reference

## Introduction
The LayerVault API provides a simple HTTP based REST based mechanism for interacting with LayerVault and allowing applications to access user information via oAuth 2 authenticated requests, over SSL. API responses are simple JSON objects.

## Getting Started

0. Register an API client from your Account Settings under the Applications menu.
0. For the Redirect URI, use ```urn:ietf:wg:oauth:2.0:oob``` for now. Later on, if you were building a Web application for an API client, you would specify your post-user authorization redirect URL here.
0. Note your ```client_id``` and ```client_secret```.
0. Try the example Access Token request below.

### API Endpoint
https://layervault.com/api/v1

### Summary of Resource URL Patterns

    /v1/me
    /v1/keypair(.:format)
    /v1/:organization_name
    /v1/:organization_name/:project
    /v1/:organization_name/:project/move
    /v1/:organization_name/:project/color
    /v1/:organization_name/:project(/folder_path)
    /v1/:organization_name/:project(/folder_path)/move
    /v1/:organization_name/:project(/folder_path)/color
    /v1/:organization_name/:project(/folder_path)/:file_name
    /v1/:organization_name/:project(/folder_path)/:file_name/move
    /v1/:organization_name/:project(/folder_path)/:file_name/sync_check
    /v1/:organization_name/:project(/folder_path)/:file_name/:revision
    /v1/:organization_name/:project(/folder_path)/:file_name/:revision/previews
    /v1/:organization_name/:project(/folder_path)/:file_name/:revision/revisions
    /v1/:organization_name/:project(/folder_path)/:file_name/:revision/meta

### URL Parameters
    :organization_name  - The URL encoded name of the Organization. Cannot be blank.
    :project            - The URL encoded name of the Project. Cannot be blank.
    :folder_path        - An optional path to a folder within a Project.
    :file_name          - The URL encoded name of the File.
    :revision           - An integer representing the Revision.

## Authentication and Requesting Access Tokens

All API requests are required to be authenticated via an oAuth 2 access token. It's easy to request a token via the command line to get a feel for how things work:

```shell
    curl -i https://layervault.com/oauth/token \
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

### Configuring A Web Application as a LayerVault API client

First of all, make sure that your redirect URI is correctly specified under your Application settings under your Layervault Account settings.

Then, when configuring your oAuth 2 client library, tell it to use ```https://layervault.com/oauth/authorize``` to request authorization and ```https://layervault.com/oauth/token``` to get access tokens. Most libraries will default to this convention anyway.

If you're building a Rails application and using OmniAuth, we have a pre-baked Authentication Strategy for you [here](https://github.com/layervault/omniauth-layervault).

### Refresh Tokens

The LayerVault API implements Refresh Tokens, allowing you to request a new access token using the refresh token, thus saving the user from having to re-authorize your application. The Refresh token is provided when the Access token is requested, along with the expiration time of the Access token, before which your refresh token should be used.

## Making API Calls

Once you have your access token, making some API calls from the command line is also easy:

```shell
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

## Methods

### Key Pairs

#### Retrieving Key Pairs

This call returns a set of KeyPairs for future uploading of files.

 Definition

    GET /api/v1/keypair

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/keypair'

 Example Response

```json
    {
      "access_key_id": "AKIAIMLDM23CBAOOEXNQ",
      "secret_access_key": "/fPHLA5505fwgvxc2gsbYd19HoEBZHpRoaj4U3G"
    }
```

#### Arguments
No arguments are required.

#### Returns

- HTTP Status: 200

Returns a JSON object containing:

  - ```access_key_id``` - The Access key ID
  - ```secret_access_key``` - The Secret Access Key

### User Information

#### Retrieving Organization Information

This call returns the user information for which the Client is acting on behalf of, including the Projects the user has access to and Organizations they are a member of.

 Definition

    GET /api/v1/me

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/me'

 Example Response

```json
{
  "email": "john@layervault.com",
  "first_name": "John",
  "last_name": "McDowall",
  "is_admin": true,
  "projects": [
    {
      "name": "Folder",
      "deleted_at": null,
      "updated_at": "2013-10-25T22:52:42Z",
      "color": null,
      "organization_name": "Acme Co",
      "organization_permalink": "acme-co"
    }
  ],
  "organizations": [
    {
      "name": "Acme Co",
      "deleted_at": "3013-01-01T00:00:00Z",
      "updated_at": "2013-10-25T22:52:44Z",
      "full_url": "https://layervault.com/acme-co",
      "projects": [
        {
          "name": "Folder",
          "deleted_at": null,
          "updated_at": "2013-10-25T22:52:44Z",
          "color": null,
          "organization_name": "Acme Co",
          "organization_permalink": "acme-co"
        }
      ]
    }
  ]
}
```

#### Arguments
No arguments are required.

#### Returns

Returns a JSON object containing:

  - ```email``` - The email address of the user.
  - ```first_name``` - The First name of the user.
  - ```last_name``` - The Last name of the user.
  - ```is_admin``` - Whether the user is an admin of the site or not.

### Organization

#### Retrieving Organization Information

This call returns the organization information for the requested Organization.

 Definition

    GET /api/v1/:organization_name

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault'

 Example Response

```json
{
  "name":"LayerVault",
  "deleted_at":"3013-02-26T16:53:38Z",
  "updated_at":"2013-10-22T00:11:57Z",
  "full_url":"https://layervault.com/layervault",
  "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
}
```

#### Arguments
No arguments are required.

#### Returns

- HTTP Status: 200

Returns a JSON object containing:

  - ```name``` - The full name of the Organization.
  - ```deleted_at``` - The date the organization was deleted (Not deleted if ```3013-02-26T16:53:38Z```)
  - ```full_url``` - The full URL to access the Organization.
  - ```projects``` - An array of the projects that the user is able to see or are public for that Organization.

### Projects

#### Retrieving Project Information

Returns the project information, including files and folders.

 Definition

    GET /api/v1/:organization_name/:project

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Designer%20News'

 Example Response

```json
{
   "full_url": "https://layervault.com/layervault/Designer%20News",
   "local_path": "~/LayerVault/Designer News",
   "md5": null,
   "organization_permalink": "layervault",
   "path": "LayerVault/Designer News",
   "shortened_url": "http://lyrv.lt/C1qWwoTZJR",
   "tree_folder_id": 2522,
   "updated_at": "2013-10-21T21:38:27Z"
   "color": "red",
   "deleted_at": null,
   "files": [
       {
           "deleted_at": null,
           "download_url": "https://layervault.com/files/download_node/QRuna998DR",
           "full_url": "https://layervault.com/layervault/Designer%20News/Front%20Page.psd",
           "local_path": "~/LayerVault/Designer News/Front Page.psd",
           "md5": "233b66b5501c71e09474ba1dac6ea91b",
           "modified_at": "2013-08-21T13:06:15Z",
           "revision_number": 290,
           "shortened_url": "http://lyrv.lt/WjB4T365vP",
           "updated_at": "2013-09-10T16:28:29Z"
       },
       ...
   ],
   "folders": [
       {
           "color": null,
           "deleted_at": null,
           "files": [
               {
                   "deleted_at": null,
                   "download_url": "https://layervault.com/files/download_node/V7x1XZnMTV",
                   "full_url": "https://layervault.com/layervault/Designer%20News/Illustrations/PageBreaks.ai",
                   "local_path": "~/LayerVault/Designer News/Illustrations/PageBreaks.ai",
                   "md5": "b1c0ee28cd5e91fe36fa55e37467edb3",
                   "modified_at": "2013-10-05T00:22:31Z",
                   "revision_number": 10,
                   "revisions": [
                       {
                           "created_at": "2013-10-05T00:09:16Z",
                           "download_url": "https://layervault.com/files/download_node/d3G8PLyj0Z",
                           "full_url": "https://layervault.com/layervault/Designer%20News/Illustrations/PageBreaks.ai/1",
                           "md5": "e309c39f50f6ad7469beb52bbdae11d0",
                           "revision_number": 1,
                           "shortened_url": "http://lyrv.lt/d3G8PLyj0Z",
                           "updated_at": "2013-10-18T18:40:12Z"
                       },
                       ...
                   ],
                   "shortened_url": "http://lyrv.lt/Jm3nbPXCc7",
                   "updated_at": "2013-10-18T18:40:12Z"
               },
               ...
           ],
           "folders": [],
           "full_url": "https://layervault.com/layervault/Designer%20News/Illustrations",
           "local_path": "~/LayerVault/Designer News/Illustrations",
           "md5": null,
           "organization_permalink": null,
           "path": "LayerVault/Designer News/Illustrations",
           "shortened_url": "http://lyrv.lt/49N7NOomDO",
           "updated_at": "2013-10-18T18:40:12Z"
       },
       ...
   ]
}
```

#### Arguments
The :organization_name and :project are required in the call URL.

#### Returns

- HTTP Status: 200

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the project
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the Project
  - ```organization_permalink``` - The URL safe permalink to the Organization
  - ```path``` - The Organization path to the Project
  - ```shortened_url``` - The shortened URL for the Project
  - ```updated_at``` - The updated at date for the projects
  - ```deleted_at``` - The deletion date for the projects
  - ```Folders``` - A JSON Array of the folders within the Project
  - ```Files``` - A JSON Array of the files within the Project

#### Creating a Project

Creates a Project under the referenced Organization.

 Definition

    POST /api/v1/:organization_name/:project

 Example Request

    $ curl -X POST -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test'

 Example Response

```json
{
  "path":"LayerVault/Test",
  "local_path":"~/LayerVault/Test",
  "color":null,
  "updated_at":"2013-10-21T19:05:09Z",
  "deleted_at":null,
  "md5":null,
  "full_url":"https://layervault.com/layervault/Test",
  "shortened_url":"http://lyrv.lt/ITtsnR2Gd0",
  "organization_permalink":"layervault",
  "folders":[],
  "files":[]
}
```

#### Arguments
The :organization_name and :project are required in the call URL.

#### Returns

- HTTP Status: 200
- HTTP Header ```Location``` containing the URL by which the newly created project can be interrogated.

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the Folder
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the Folder
  - ```organization_permalink``` - The URL safe permalink to the Organization
  - ```path``` - The Organization path to the Folder
  - ```shortened_url``` - The shortened URL for the Folder
  - ```updated_at``` - The updated at date for the Folders
  - ```deleted_at``` - The deletion date for the Folders
  - ```Folders``` - A JSON Array of the folders within the Folder
  - ```Files``` - A JSON Array of the files within the Folder

#### Deleting a Project

Deletes a Project folder. Returns an HTTP 200 on success.

 Definition

    DELETE /api/v1/:organization_name/:project

 Example Request

    $ curl -X DELETE -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test'

 Example Response

    None.

#### Arguments
The :organization_name and :project are required in the call URL.

#### Returns

- HTTP Status: 200

#### Moving a Project

Moves a project to a new location within the referenced Organization.

 Definition

    POST /api/v1/:organization_name/:project/move

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/test/move'

 Example Response

```json
{
  "path": "LayerVault/api-playground/VictimMove",
  "local_path": "~/LayerVault/api-playground/VictimMove",
  "color": null,
  "updated_at": "2013-10-21T19:05:32Z",
  "deleted_at": null,
  "md5": null,
  "full_url": "https://layervault.com/layervault/api-playground/VictimMove",
  "shortened_url": "http://lyrv.lt/VPqZQNopEu",
  "organization_permalink": null,
  "folders": [],
  "files": []
}
```

#### Arguments
The :organization_name and :project are required in the call URL. A POST parameter of :to shoud be supplied representing the destination Folder to move the current Folder to.

#### Returns

- HTTP Status: 200 on success.

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the project
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the Project
  - ```organization_permalink``` - The URL safe permalink to the Organization
  - ```path``` - The Organization path to the Project
  - ```shortened_url``` - The shortened URL for the Project
  - ```updated_at``` - The updated at date for the projects
  - ```deleted_at``` - The deletion date for the projects
  - ```Folders``` - A JSON Array of the folders within the Project
  - ```Files``` - A JSON Array of the files within the Project

#### Changing a Project's color

Changes the color of the Project color as it appears in the LayerVault web Application and locally on the user's file system.

 Definition

    PUT /api/v1/:organization_name/:project

 Example Request

    $ curl -X PUT -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test'

 Example Response

```json
{
  "name":"LayerVault",
  "deleted_at":"3013-02-26T16:53:38Z",
  "updated_at":"2013-10-22T00:11:57Z",
  "full_url":"https://layervault.com/layervault",
  "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
}
```

#### Arguments
The :organization_name and :project are required in the call URL. A PUT parameter of :color shoud be supplied representing the color that the destination Folder should be set to.

#### Returns

- HTTP Status: 200 on success.

### Folders

#### Retrieving Folders's Information

This call returns the organization information for the requested Organization.

 Definition

    GET /api/v1/:organization_name/:project/:folder_path

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations'

 Example Response

```json
{
  "path": "LayerVault/test/Illustrations",
  "local_path": "~/LayerVault/test/Illustrations",
  "color": null,
  "updated_at": "2013-10-21T19:05:39Z",
  "deleted_at": null,
  "md5": null,
  "full_url": "https://layervault.com/layervault/test/Illustrations",
  "shortened_url": "http://lyrv.lt/K75jvYYhXK",
  "organization_permalink": null,
  "folders": [],
  "files": []
}
```

#### Arguments
The :organization_name, :project and :folder_path are required in the call URL.

#### Returns

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the project
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the Folder
  - ```organization_permalink``` - The URL safe permalink to the Organization
  - ```path``` - The Organization path to the Folder
  - ```shortened_url``` - The shortened URL for the Folder
  - ```updated_at``` - The updated at date for the Folder
  - ```deleted_at``` - The deletion date for the Folder
  - ```Folders``` - A JSON Array of the folders within the Folder
  - ```Files``` - A JSON Array of the files within the Folder

#### Creating a Folder

Creates a Folder at the given Folder path under the referenced Organization. Folder paths can be nested as many levels as is necessary.

 Definition

    POST /api/v1/:organization_name/:project/:folder_path

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations'

 Example Response

```json
{
  "path":"LayerVault/Test/Illustrations",
  "local_path":"~/LayerVault/Test/Illustrations",
  "color":null,
  "updated_at":"2013-10-21T19:05:09Z",
  "deleted_at":null,
  "md5":null,
  "full_url":"https://layervault.com/layervault/Test/Illustrations",
  "shortened_url":"http://lyrv.lt/ITtsnR2Gd0",
  "organization_permalink":"layervault",
  "folders":[],
  "files":[]
}
```

#### Arguments
The :organization_name, :project and :folder_path are required in the call URL.

#### Returns

- HTTP Status: 200 on success.
- HTTP Header ```Location``` containing the URL by which the newly created Folder can be interrogated.

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the Folder
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the Folder
  - ```organization_permalink``` - The URL safe permalink to the Organization
  - ```path``` - The Organization path to the Folder
  - ```shortened_url``` - The shortened URL for the Folder
  - ```updated_at``` - The updated at date for the Folders
  - ```deleted_at``` - The deletion date for the Folders
  - ```Folders``` - A JSON Array of the folders within the Folder
  - ```Files``` - A JSON Array of the files within the Folder

#### Deleting a Folder

Deletes the requested folder under the references Organization.

 Definition

    DELETE /api/v1/:organization_name/:project/:folder_path

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations'

 Example Response

    None

#### Arguments
The :organization_name, :project and :folder_path are required in the call URL.

#### Returns

- Http Response: 200 on success

#### Moving a Folder

Moves a folder to a new specified location under the referenced Organization

 Definition

    POST /api/v1/:organization_name/:project/:folder_path/move

 Example Request

    $ curl -X POST -d 'to=/NewTest/NewIllustrations' -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/move'

 Example Response

```json
{
  "path": "LayerVault/api-playground/VictimMove",
  "local_path": "~/LayerVault/api-playground/VictimMove",
  "color": null,
  "updated_at": "2013-10-21T19:05:32Z",
  "deleted_at": null,
  "md5": null,
  "full_url": "https://layervault.com/layervault/api-playground/VictimMove",
  "shortened_url": "http://lyrv.lt/VPqZQNopEu",
  "organization_permalink": null,
  "folders": [],
  "files": []
}
```

#### Arguments
The :organization_name, :project and :folder_path are required in the call URL. A POST parameter of :to is required to specify the destination location under the referenced Organization that the folder is to be moved to.

#### Returns

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the project
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the Project
  - ```organization_permalink``` - The URL safe permalink to the Organization
  - ```path``` - The Organization path to the Project
  - ```shortened_url``` - The shortened URL for the Project
  - ```updated_at``` - The updated at date for the projects
  - ```deleted_at``` - The deletion date for the projects
  - ```Folders``` - A JSON Array of the folders within the Project
  - ```Files``` - A JSON Array of the files within the Project

#### Changing a Folder's color

This call returns the organization information for the requested Organization.

 Definition

    PUT /api/v1/:organization_name/:project/:folder_path

 Example Request

    $ curl -X PUT -d "color=000000" -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations'

 Example Response

    None

#### Arguments
The :organization_name, :project and :folder_path are required in the call URL. A PUT parameter :color representing the Hex color value the folder should be set to is required.

#### Returns

- HTTP Response: 200 on success.

### Files

#### Retrieving Files's Information

Returns a referenced File's information.

 Definition

    GET /api/v1/:organization_name/:project/:folder_path/:file_name

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/NewLogo.psd'

 Example Response

```json
{
  "download_url": "https://layervault.com/files/download_node/2YDJVhvxLV",
  "full_url": "https://layervault.com/layervault/Test/Illustrations/NewLogo.psd/1",
  "md5": "837b0a406b101620a3d2b33867d66560",
  "updated_at": "2013-10-21T19:05:23Z",
  "created_at": "2013-10-21T19:05:23Z",
  "shortened_url": "http://lyrv.lt/2YDJVhvxLV",
  "revision_number": 1
}
```

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL.

#### Returns

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the File
  - ```download_url``` - The absolute URL to download a copy of the File
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the File
  - ```shortened_url``` - The shortened URL for the Folder
  - ```updated_at``` - The updated at date for the Folders
  - ```deleted_at``` - The deletion date for the Folders
  - ```revision_number``` - The revision number of the File

#### Creating a File

Creates a File under the referenced folder path and organization.

 Definition

    PUT /api/v1/:organization_name/:project/:folder_path/:file_name

 Example Request

    $ curl -X PUT -d 'md5=837b0a406b101620a3d2b33867d66560&remote_url=http://url/to/remote/file' -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd'

 Example Response

```json
{
  "download_url": "https://layervault.com/files/download_node/2YDJVhvxLV",
  "full_url": "https://layervault.com/layervault/Test/Illustrations/NewLogo.psd/1",
  "md5": "837b0a406b101620a3d2b33867d66560",
  "updated_at": "2013-10-21T19:05:23Z",
  "created_at": "2013-10-21T19:05:23Z",
  "shortened_url": "http://lyrv.lt/2YDJVhvxLV",
  "revision_number": 1
}
```


#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL.

#### Returns

- HTTP Status: 200 on success.

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the File
  - ```download_url``` - The absolute URL to download a copy of the File
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the File
  - ```shortened_url``` - The shortened URL for the Folder
  - ```updated_at``` - The updated at date for the Folders
  - ```deleted_at``` - The deletion date for the Folders
  - ```revision_number``` - The revision number of the File

#### Deleting a File

Deletes a referenced File.

 Definition

    DELETE /api/v1/:organization_name/:project/:folder_path/:file_name

 Example Request

    $ curl -X DELETE -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd'

 Example Response

    None

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL.

#### Returns

  - HTTP Response: 200 on success.

#### Moving a File

Moves a File to a new Folder and optionally a new Filename under a referenced Organization.

 Definition

    POST /api/v1/:organization_name/:project/:folder/:file_name

 Example Request

    $ curl -X POST -D 'new_folder=/this/is/the/new/folder&new_filename=bert.psd' -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/Test.psd'

 Example Response

```json
{
  error: 'success'
}
```

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL.

#### Returns

  - HTTP Status: 200 on success

Returns a JSON object containing:

  - ```error``` - Any errors that may have prevented the File from being moved, or 'success' if successful.

#### Performing a Sync check on a File

Indicates whether a file needs to be uploaded in full because the server doesn't have a copy, partial upload if a file has changed, or if the file needs to be deleted.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/sync_check

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/Test.psd/sync_check'

 Example Response

```json
{
  "name":"LayerVault",
  "deleted_at":"3013-02-26T16:53:38Z",
  "updated_at":"2013-10-22T00:11:57Z",
  "full_url":"https://layervault.com/layervault",
  "projects":[{"name":"api-playground","deleted_at":"3013-01-01T00:00:00Z","updated_at":"2013-10-21T19:05:40Z","color":null}]
}
```

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL. The :md5 GET parameter is required, and either the :file_data or :remote_url parameters.

#### Returns

Returns a JSON object containing:

  - HTTP Status: 200 - Upload the full file
  - HTTP Status: 409 - No need to upload anything
  - HTTP Status: 400 - File Size was missing
  - HTTP Status: 413 - The file is too big
  - HTTP Status: 412 - The file should be uploaded in full.

### Revisions

#### Retrieving Revisions' Information

This call returns the organization information for the requested Organization.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd/1'

 Example Response

```json
{
  "download_url": "https://layervault.com/files/download_node/DwoRKvzbEj",
  "full_url": "https://layervault.com/layervault/api-playground/Test.psd/1",
  "md5": "e388875b2d81f6798dd187d1c047a50c",
  "updated_at": "2013-10-21T19:05:24Z",
  "created_at": "2013-10-21T19:05:24Z",
  "shortened_url": "http://lyrv.lt/DwoRKvzbEj",
  "revision_number": 1
}
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revisions are required in the call URL.

#### Returns

Returns a JSON object containing:

  - ```full_url``` - The absolute URL to the File
  - ```download_url``` - The absolute URL to download a copy of the File
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the File
  - ```shortened_url``` - The shortened URL for the Folder
  - ```updated_at``` - The updated at date for the Folders
  - ```deleted_at``` - The deletion date for the Folders
  - ```revision_number``` - The revision number of the File

#### Retrieving Revision's Revision History

This call returns the organization information for the requested Organization.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/revisions

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervaultTest/Illustrations/NewFile.psd/1/revisions'

 Example Response

```json
[
  {
    "download_url": "https://layervault.com/files/download_node/vKUNqi6jFi",
    "full_url": "https://layervault.com/layervault/api-playground/Test.psd/2",
    "md5": "65ef424c001b078516d953f1e4a66450",
    "updated_at": "2013-10-21T19:05:25Z",
    "created_at": "2013-10-21T19:05:25Z",
    "shortened_url": "http://lyrv.lt/vKUNqi6jFi",
    "revision_number": 2
  },
  {
    "download_url": "https://layervault.com/files/download_node/udMqnVagH6",
    "full_url": "https://layervault.com/layervault/api-playground/Test.psd/3",
    "md5": "4edea58eacd8c9334e4df173dad72d69",
    "updated_at": "2013-10-21T19:05:27Z",
    "created_at": "2013-10-21T19:05:27Z",
    "shortened_url": "http://lyrv.lt/udMqnVagH6",
    "revision_number": 3
  }
]
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revisions are required in the call URL.

#### Returns

Returns a JSON array containing objects with the following attributes:

  - ```full_url``` - The absolute URL to the File
  - ```download_url``` - The absolute URL to download a copy of the File
  - ```local_path``` - The local path on the user's filesystem
  - ```md5``` - The MD5 hash of the File
  - ```shortened_url``` - The shortened URL for the Folder
  - ```updated_at``` - The updated at date for the Folders
  - ```deleted_at``` - The deletion date for the Folders
  - ```revision_number``` - The revision number of the File

#### Retrieving Revision's Meta Information

This call returns the organization information for the requested Organization.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/meta

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd/1/meta'

 Example Response

```json
{
  "key": "value"
}
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revisions are required in the call URL.

#### Returns

Returns a JSON object containing any Meta information that was set for the revision.

#### Retrieving Revision's Previews Information

This call returns the organization information for the requested Organization.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/previews

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://layervault.com/api/v1/layervaultTest/Illustrations/NewFile.psd/1/previews'

 Example Response

```json
[]
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revisions are required in the call URL.

#### Returns

Returns a JSON array containing a list of preview image URLs.
