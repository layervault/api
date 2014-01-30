### Projects

#### Retrieving Project Information

Returns the project information, including files and folders.

 Definition

    GET /api/v1/:organization_name/:project

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://api.layervault.com/api/v1/layervault/Designer%20News'

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

  - `full_url` - The absolute URL to the project
  - `local_path` - The local path on the user's filesystem
  - `md5` - The MD5 hash of the Project
  - `organization_permalink` - The URL safe permalink to the Organization
  - `path` - The Organization path to the Project
  - `shortened_url` - The shortened URL for the Project
  - `updated_at` - The updated at date for the projects
  - `deleted_at` - The deletion date for the projects
  - `folders` - A JSON Array of the folders within the Project
  - `files` - A JSON Array of the files within the Project

#### Creating a Project

Creates a Project under the referenced Organization.

 Definition

    POST /api/v1/:organization_name/:project

 Example Request

  ```
  $ curl -X POST \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test'
  ```

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

  - `full_url` - The absolute URL to the Folder
  - `local_path` - The local path on the user's filesystem
  - `md5` - The MD5 hash of the Folder
  - `organization_permalink` - The URL safe permalink to the Organization
  - `path` - The Organization path to the Folder
  - `shortened_url` - The shortened URL for the Folder
  - `updated_at` - The updated at date for the Folders
  - `deleted_at` - The deletion date for the Folders
  - `folders` - A JSON Array of the folders within the Folder
  - `files` - A JSON Array of the files within the Folder

#### Deleting a Project

Removes the User from the project.

Definition

    DELETE /api/v1/:organization_name/:project

Example Request

```
$ curl -X DELETE \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test'
```

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

```
$ curl -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/test/move'
```

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

  - `full_url` - The absolute URL to the project
  - `local_path` - The local path on the user's filesystem
  - `md5` - The MD5 hash of the Project
  - `organization_permalink` - The URL safe permalink to the Organization
  - `path` - The Organization path to the Project
  - `shortened_url` - The shortened URL for the Project
  - `updated_at` - The updated at date for the projects
  - `deleted_at` - The deletion date for the projects
  - `folders` - A JSON Array of the folders within the Project
  - `files` - A JSON Array of the files within the Project

#### Changing a Project's color

Changes the color of the Project color as it appears in the LayerVault web Application and locally on the user's file system.

Definition

    PUT /api/v1/:organization_name/:project

Example Request

```shell
$ curl -X PUT \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test'
```

Example Response

```json
{
  "name":"LayerVault",
  "deleted_at":null,
  "updated_at":"2013-10-22T00:11:57Z",
  "full_url":"https://layervault.com/layervault",
  "projects": [
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
The :organization_name and :project are required in the call URL. A PUT parameter of :color shoud be supplied representing the color that the destination Folder should be set to.

#### Returns

- HTTP Status: 200 on success.
