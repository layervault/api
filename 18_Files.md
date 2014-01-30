### Files

#### Retrieving Files's Information

Returns a referenced File's information.

Definition

    GET /api/v1/:organization_name/:project/:folder_path/:file_name

Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/NewLogo.psd'
```

Example Response

```json
{
  "local_path": "~/LayerVault/Test/Illustrations/NewLogo.psd",
  "name": "NewLogo.psd",
  "deleted_at": null,
  "updated_at": "2013-10-21T19:05:23Z",
  "modified_at": "2013-10-21T19:05:23Z",
  "md5": "837b0a406b101620a3d2b33867d66560",
  "download_url": "https://layervault.com/files/download_node/2YDJVhvxLV",
  "full_url": "https://layervault.com/layervault/Test/Illustrations/NewLogo.psd",
  "shortened_url": "http://lyrv.lt/2YDJVhvxLV",
  "revision_number": 1,
  "revisions": []
}
```

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL.

#### Returns

Returns a JSON object containing:

  - `local_path` - The local path on the user's filesystem
  - `name` - The name of the file
  - `deleted_at` - When the file was deleted. Returns null if not deleted.
  - `updated_at` - The updated at date for the file
  - `modified_at` - The date the client modified the file
  - `full_url` - The absolute URL to the File
  - `download_url` - The absolute URL to download a copy of the File
  - `md5` - The MD5 hash of the File
  - `shortened_url` - The shortened URL for the Folder
  - `revision_number` - The revision number of the File
  - `revisions` - Array of Revision objects

#### Creating and then Uploading a File

Creates a File under the referenced folder path and organization. Returns a JSON payload containing the necessary S3 upload parameters to make an file upload directly to the LayerVault servers.

High Level Overview of Request Flow

  1. Request the S3 JSON upload parameters from LayerVault via a `PUT` request with an `md5` parameter of the file you want to upload
  2. Use those parameters to make a **multipart** POST with the file directly to S3. If you see XML in the response to this request you have offended Amazon S3. Don't alter, change or add any of the parameters you were given in Step 1.
  3. If all is well, S3 will respond with a callback URL in the `Location` header of the response to the request you just made in Step 2.
  4. Take that URL from the `Location` header and attach your access_token to the end of the query string and make a POST request to that URL. If you receive a 401 at this stage it's because you have forgotten to add the access_token (you're basically making a normal API request in this step).
  5. LayerVault will receive the POST request and begin to process your upload internally.

Definition

    PUT /api/v1/:organization_name/:project/:folder_path/:file_name

Example Request

```shell
$ curl -X PUT \
    -d 'md5=837b0a406b101620a3d2b33867d66560' \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd'
```

 Example Response

```json
{
  "key": "8021/1383269620.4031708/dhh.jpg",
  "acl": "private",
  "policy": "eyJleHBpcmF0aW9uIjoiMjAxMy0xMS0wMVQwMjozMzo0MC4wMDBaIiwiY29uZGl0aW9ucyI6W3siYnVja2V0Ijoib21uaXZvcmUtc2NyYXRjaCJ9LFsic3RhcnRzLXdpdGgiLCIka2V5IiwiIl0sWyJzdGFydHMtd2l0aCIsIiRDb250ZW50LVR5cGUiLCJpbWFnZS8iXSx7ImFjbCI6InByaXZhdGUifSx7InN1Y2Nlc3NfYWN0aW9uX3N0YXR1cyI6IjIwMCJ9LHsic3VjY2Vzc19hY3Rpb25fcmVkaXJlY3QiOiJodHRwczovL2xheWVydmF1bHQuY29tL2FwaS92MS9sYXllcnZhdWx0LXRlc3QvYXBpLXBsYXlncm91bmQvZm9sZGVyL2RoaC5qcGcvdXBsb2FkX2NvbXBsZXRlP21kNT1jNDgyZjZkYjg2OGEyMzNjMTRmZjAzYmVlMzRhNmE5NyZyZW1vdGVfdXJsPWh0dHBzOi8vczMuYW1hem9uYXdzLmNvbS9vbW5pdm9yZS1zY3JhdGNoLzgwMjEvMTM4MzI2OTYyMC40MDMxNzA4L2RoaC5qcGcifSxbImNvbnRlbnQtbGVuZ3RoLXJhbmdlIiwwLDEwNDg1NzYwMDBdXX0=",
  "signature": "IRGVgXgKclStT/uJvTI4zyF8NYI=",
  "success_action_status": 200,
  "success_action_redirect": "https://layervault.com/api/v1/layervault-test/api-playground/folder/dhh.jpg/upload_complete?md5=c482f6db868a233c14ff03bee34a6a97&remote_url=https://s3.amazonaws.com/omnivore-scratch/8021/1383269620.4031708/dhh.jpg",
  "AWSAccessKeyId": "19YE8S0YFVC4DNK2EFG2"
}
```

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL. The PUT Parameter `md5` should be included which represents the MD5 digest hash of the contents of the file you wish to upload.

#### Returns

- HTTP Status: 200 on success.

Returns a JSON object containing:

  - `key` - The S3 key of the file upload.
  - `acl` - The S3 ACL
  - `policy` - The encoded S3 Policy
  - `signature` - The HMAC signature for the S3 policy
  - `success_action_status` - The HTTP status code for the success redirect
  - `success_action_redirect` - The success redirect URL
  - `AWSAccessKeyId` - The S3 Access Key for LayerVault.

DO NOT ALTER ANY OF THESE PARAMETERS. If you do, the upload will fail.

Once you have all of these parameters, include a parameter for `Content-Type` which matches the Content Type of the file you are trying to upload. Finally, include a `file` parameter that is a `binary multipart` representation of your file you wish to upload, and make a `POST` request with all of these parameters to `https://omnivore-scratch.s3.amazonaws.com`. Do not change, alter, or add anything to these parameters outlined as you will receive nasty XML error messages from Amazon S3.

NB: Make sure the `file` parameter is the last in the above list of parameters - S3 ignores `POST` fields that come after the `file` parameter.

Upon success, you will receive a response from Amazon S3 with a `Location` header with a URL to which you should make a POST request verbatim, with the inclusion of your access token to the query string. Once that call completes, your file will be processed and ready for display on LayerVault at the location you specified.

#### Deleting a File

Deletes a referenced File.

Definition

    DELETE /api/v1/:organization_name/:project/:folder_path/:file_name

Example Request

```shell
$ curl -X DELETE \
    -d 'md5=837b0a406b101620a3d2b33867d66560' \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd'
```

 Example Response

    None

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL. A POST parameter `:md5` is required and must match the MD5 of the file to be deleted.

#### Returns

  - HTTP Response: 200 on success.

#### Moving a File

Moves a File to a new Folder and optionally a new Filename under a referenced Organization.

Definition

    POST /api/v1/:organization_name/:project/:folder/:file_name/move

Example Request

```shell
$ curl -X POST \
    -d 'to=/this/is/the/new/folder&new_file_name=bert.psd' \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/Test.psd/move'
```

Example Response

```json
{
  error: 'success'
}
```

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL. The `to` param is required and must specify the destination folder relative to the organization root. If you wish to rename the file as well, you can give the `new_file_name` parameter.

#### Returns

  - HTTP Status: 200 on success

Returns a JSON object containing:

  - `error` - Any errors that may have prevented the File from being moved, or 'success' if successful.

#### Retrieving File's Revision History

Returns a referenced File Revisions Revision History.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/revisions

 Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervaultTest/Illustrations/NewFile.psd/revisions'
```

 Example Response

```json
[
  {
    "name": "1",
    "download_url": "https://layervault.com/files/download_node/vKUNqi6jFi",
    "full_url": "https://layervault.com/layervault/api-playground/Test.psd/2",
    "md5": "65ef424c001b078516d953f1e4a66450",
    "deleted_at": null,
    "updated_at": "2013-10-21T19:05:25Z",
    "created_at": "2013-10-21T19:05:25Z",
    "shortened_url": "http://lyrv.lt/vKUNqi6jFi",
    "revision_number": 1,
    "user": {},
    "previews": []
  },
  {
    "name": "2",
    "download_url": "https://layervault.com/files/download_node/udMqnVagH6",
    "full_url": "https://layervault.com/layervault/api-playground/Test.psd/3",
    "md5": "4edea58eacd8c9334e4df173dad72d69",
    "deleted_at": null,
    "updated_at": "2013-10-21T19:05:27Z",
    "created_at": "2013-10-21T19:05:27Z",
    "shortened_url": "http://lyrv.lt/udMqnVagH6",
    "revision_number": 2,
    "user": {},
    "previews": []
  }
]
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revisions are required in the call URL.

#### Returns

Returns a JSON array containing objects with the following attributes:

  - `name` - The name of the revision, which is the same as it's revision number
  - `full_url` - The absolute URL to the Revision
  - `download_url` - The absolute URL to download a copy of the Revision
  - `md5` - The MD5 hash of the Revision
  - `shortened_url` - The shortened URL for the Revision
  - `updated_at` - The updated at date for the Revision
  - `deleted_at` - The deletion date for the Revision
  - `created_at` - The creation date for the Revision
  - `revision_number` - The revision number for the revision
  - `user` - The user who created the Revision
  - `previews` - All previews generated for the Revision

#### Retrieving a File's Preview Information

Returns all of the previews associated with the latest revision of a file.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/previews

 Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervaultTest/Illustrations/NewFile.psd/1/previews'
```

 Example Response

```json
[
  'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?s=6666868519b243260d38b3ca1d1b23ef',
  'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?s=6666868519b243260d38b3ca1d1b4edf',
  'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?s=6666868519b243260d38b3ca1d1bw21f',
...
]
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name are required in the call URL. In addition, you can add GET parameters that will be passed along to imgix for formatting the image. See [their API documentation](http://www.imgix.com/docs/urlapi) for available options.

#### Returns

Returns a JSON array containing a list of preview image URLs.

#### Retrieving the Main Preview for a File

Returns the primary preview for the file, which is the first page in multi-page documents.

Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/preview

Example Request

``` shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervaultTest/Illustrations/NewFile.psd/1/preview'
```

Example Response

``` json
'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?s=6666868519b243260d38b3ca1d1b23ef'
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name are required in the call URL. In addition, you can add GET parameters that will be passed along to imgix for formatting the image. See [their API documentation](http://www.imgix.com/docs/urlapi) for available options.

#### Returns

The URL to the preview.

#### Retreiving a File's Feedback Items

Obtain the feeback items attached to a file.

Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/feedback_items

Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervault/Project/Test.psd/feedback_items'
```

Example Response

```json
[
  {
    "id": 1687,
    "top": null,
    "right": null,
    "left": null,
    "bottom": null,
    "left_on_preview_id": 436729,
    "message": "I meant to say Front Page, not Story Page :(",
    "created_at": "2013-04-19T19:06:11Z"
    "user": {},
    "addressed_by": null,
    "replies": []
  }
]
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revision are required in the call URL.

#### Returns

Returns a JSON array of objects containing:

  - `id` - The ID of the feedback item.
  - `top` - The top coordinate of the feedback annotation bounding box.
  - `right` - The right coordinate of the feedback annotation bounding box.
  - `left` - The left coordinate of the feedback annotation bounding box.
  - `bottom` - The bottom coordinate of the feedback annotation bounding box.
  - `left_on_preview_id` - The preview id the feedback item was left on.
  - `message` - The text of the feedback item.
  - `created_at` - The date the feedback item was created.
  - `user` - User object for the user who left the feedback.
  - `addressed_by` - The feedback item that was created when Requesting Team Feedback and marks this feedback item as addressed.
  - `replies` - Array of feedback item objects that are a reply to this feedback item. Threads can only be 1 level deep, so the replies will not have replies.

#### Performing a Sync check on a File

Indicates whether a file needs to be uploaded in full because the server doesn't have a copy, partial upload if a file has changed, or if the file needs to be deleted.

Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/sync_check

Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervault/Project/Test.psd/sync_check'
```

 Example Response

```json
{
  "name":"LayerVault",
  "deleted_at":null,
  "updated_at":"2013-10-22T00:11:57Z",
  "full_url":"https://layervault.com/layervault",
  "projects":[
    {
      "name":"api-playground",
      "deleted_at":null,
      "updated_at":"2013-10-21T19:05:40Z",
      "color":null
    }
  ]
}
```

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL. The :md5 GET parameter is required, and either the :file_data or :remote_url parameters.

#### Returns

Returns a JSON object containing:

  - HTTP Status: 200 - Upload the full file
  - HTTP Status: 409 - File is old and needs to be downloaded
  - HTTP Status: 400 - File Size was missing
  - HTTP Status: 413 - The file is too big
  - HTTP Status: 412 - The file should be uploaded in full.
