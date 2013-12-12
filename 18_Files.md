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

  - `full_url` - The absolute URL to the File
  - `download_url` - The absolute URL to download a copy of the File
  - `local_path` - The local path on the user's filesystem
  - `md5` - The MD5 hash of the File
  - `shortened_url` - The shortened URL for the Folder
  - `updated_at` - The updated at date for the Folders
  - `deleted_at` - The deletion date for the Folders
  - `revision_number` - The revision number of the File

#### Creating and then Uploading a File

Creates a File under the referenced folder path and organization. Returns a JSON payload containing the necessary S3 upload parameters to make an file upload directly to the LayerVault servers.

High Level Overview of Request Flow

  1. Request the S3 JSON upload parameters from LayerVault via a ```PUT``` request with an ```md5``` parameter of the file you want to upload
  2. Use those parameters to make a Multi-part POST with the file directly to S3. If you see XML in the response to this request you have offended Amazon S3. Don't alter, change or add any of the parameters you were given in Step 1.
  3. For a properly formed request in Step 2, S3 will respond with a callback URL in the ```Location``` header of the response to the request you just made in Step 2.
  4. Take that URL from the ```Location``` header and attach your access_token to the end of the query string and make a POST request to that URL. If you receive a 401 at this stage it's because you have forgotten to add the access_token (you're basically making a normal API request in this step).
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
The :organization_name, :project, :folder_path and :file_name are required in the call URL. The PUT Parameter ```md5``` should be included which represents the MD5 digest hash of the contents of the file you wish to upload.

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

Once you have all of these parameters, include a parameter for "Content-Type" which matches the Content Type of the file you are trying to upload. Finally, include a ```file``` parameter that is a ```binary multi-part``` representation of your file you wish to upload, and make a ```POST``` request with all of these parameters to ```https://omnivore-scratch.s3.amazonaws.com```. Do not change, alter, or add anything to these parameters outlined as you will receive nasty XML error messages from Amazon S3.

NB: Make sure the ```file``` parameter is the last in the above list of parameters - S3 ignores ```POST``` fields that come after the ```file``` parameter.

Upon success, you will receive a response from Amazon S3 with a ```Location``` header with a URL to which you should make a POST request verbatim, with the inclusion of your access token to the query string. Once that call completes, your file will be processed and ready for display on LayerVault at the location you specified.

#### Deleting a File

Deletes a referenced File.

Definition

    DELETE /api/v1/:organization_name/:project/:folder_path/:file_name

Example Request

```shell
$ curl -X DELETE \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd'
```

 Example Response

    None

#### Arguments
The :organization_name, :project, :folder_path and :file_name are required in the call URL. A POST parameter ```:md5``` is required and must match the MD5 of the file to be deleted.

#### Returns

  - HTTP Response: 200 on success.

#### Moving a File

Moves a File to a new Folder and optionally a new Filename under a referenced Organization.

Definition

    POST /api/v1/:organization_name/:project/:folder/:file_name

Example Request

```shell
$ curl -X POST \
    -D 'new_folder=/this/is/the/new/folder&new_filename=bert.psd' \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/Test.psd'
```

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

  - `full_url` - The absolute URL to the File
  - `download_url` - The absolute URL to download a copy of the File
  - `local_path` - The local path on the user's filesystem
  - `md5` - The MD5 hash of the File
  - `shortened_url` - The shortened URL for the Folder
  - `updated_at` - The updated at date for the Folders
  - `deleted_at` - The deletion date for the Folders
  - `revision_number` - The revision number of the File

#### Retrieving a File's Preview Information

Returns all of the previews associated with a file.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/previews

 Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervaultTest/Illustrations/NewFile.psd/1/previews?w=100&h=100'
```

 Example Response

```json
[
  'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?w=600&h=1012&s=6666868519b243260d38b3ca1d1b23ef',
  'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?w=600&h=1012&s=6666868519b243260d38b3ca1d1b4edf',
  'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?w=600&h=1012&s=6666868519b243260d38b3ca1d1bw21f',
...
]
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name are required in the call URL. In addition, the GET parameters :w for width and :h for height must be specified.

#### Returns

Returns a JSON array containing a list of preview image URLs.

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
    "left_on_signpost_id": 3782,
    "message": "I meant to say Front Page, not Story Page :(",
    "user_id": 6,
    "signpost_id": 5636,
    "created_at": "2013-04-19T19:06:11Z"
  },
  {
    "id": 2671,
    "top": 408,
    "right": 1040,
    "left": 356,
    "bottom": 461,
    "left_on_preview_id": 436729,
    "left_on_signpost_id": 3782,
    "message": "Type choice needs some work. Mixing Avenir and Avant Garde.... not so good.",
    "user_id": 6,
    "signpost_id": 5636,
    "created_at": "2013-06-13T23:25:15Z"
  },
  {
    "id": 2672,
    "top": 445,
    "right": 2195,
    "left": 1998,
    "bottom": 668,
    "left_on_preview_id": 436729,
    "left_on_signpost_id": 3782,
    "message": "Probably need to invert these.",
    "user_id": 6,
    "signpost_id": 5636,
    "created_at": "2013-06-13T23:25:28Z"
  },
  {
    "id": 11216,
    "top": null,
    "right": null,
    "left": null,
    "bottom": null,
    "left_on_preview_id": 436729,
    "left_on_signpost_id": null,
    "message": "First draft of the story page. No story badges for the moment. Without the color coding, the black/blue starts to make my eye glaze over.",
    "user_id": 6,
    "signpost_id": null,
    "created_at": "2013-04-19T19:05:45Z"
  },
  {
    "id": 2673,
    "top": 709,
    "right": 953,
    "left": 402,
    "bottom": 804,
    "left_on_preview_id": 536981,
    "left_on_signpost_id": 5636,
    "message": "The padding here could be tightened up.",
    "user_id": 6,
    "signpost_id": null,
    "created_at": "2013-06-13T23:26:17Z"
  },
  {
    "id": 2674,
    "top": 397,
    "right": 275,
    "left": 172,
    "bottom": 493,
    "left_on_preview_id": 536981,
    "left_on_signpost_id": 5636,
    "message": "Without badges, this front page doesn't have as much personality.",
    "user_id": 6,
    "signpost_id": null,
    "created_at": "2013-06-13T23:26:38Z"
  },
  {
    "id": 2675,
    "top": 406,
    "right": 1043,
    "left": 349,
    "bottom": 454,
    "left_on_preview_id": 536981,
    "left_on_signpost_id": 5636,
    "message": "Might need to consider bringing back blue and light blue (visited) links.",
    "user_id": 6,
    "signpost_id": null,
    "created_at": "2013-06-13T23:26:57Z"
  },
  {
    "id": 13070,
    "top": null,
    "right": null,
    "left": null,
    "bottom": null,
    "left_on_preview_id": 536981,
    "left_on_signpost_id": null,
    "message": "This is probably pretty close to what will end up going to production. The padding between stories is a bit big. I think it'll mess around with row ",
    "user_id": 6,
    "signpost_id": null,
    "created_at": "2013-06-13T23:25:48Z"
  },
  {
    "id": 16772,
    "top": null,
    "right": null,
    "left": null,
    "bottom": null,
    "left_on_preview_id": 667574,
    "left_on_signpost_id": null,
    "message": "All of this was fixed.",
    "user_id": 6,
    "signpost_id": null,
    "created_at": "2013-09-10T16:28:29Z"
  }
]
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revision are required in the call URL.

#### Returns

Returns a JSON array of objects containing:

  - id - The ID of the feedback item.
  - top - The top co-ordinate of the feedback bounding box.
  - right - The right co-ordinate of the feedback bounding box.
  - left - The left co-ordinate of the feedback bounding box.
  - bottom - The bottom co-ordinate of the feedback bounding box.
  - left_on_preview_id - The preview id the feedback item was left on.
  - left_on_signpost_id - The signpost the feedback item was left on, if any.
  - message - The text of the feedback item.
  - user_id - The user ID of the user who left the feedback item.
  - signpost_id - The associated signpost ID, if any.
  - created_at - The date the feedback item was created.

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
  "deleted_at":"3013-02-26T16:53:38Z",
  "updated_at":"2013-10-22T00:11:57Z",
  "full_url":"https://layervault.com/layervault",
  "projects":[
    {
      "name":"api-playground",
      "deleted_at":"3013-01-01T00:00:00Z",
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
  - HTTP Status: 409 - No need to upload anything
  - HTTP Status: 400 - File Size was missing
  - HTTP Status: 413 - The file is too big
  - HTTP Status: 412 - The file should be uploaded in full.
