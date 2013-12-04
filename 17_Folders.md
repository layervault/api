### Folders

#### Retrieving Folders's Information

Returns the referenced Folder's information.

 Definition

    GET /api/v1/:organization_name/:project/:folder_path

 Example Request

  ```shell
  $ curl -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations'
  ```

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

  - `full_url` - The absolute URL to the project
  - `local_path` - The local path on the user's filesystem
  - `md5` - The MD5 hash of the Folder
  - `organization_permalink` - The URL safe permalink to the Organization
  - `path` - The Organization path to the Folder
  - `shortened_url` - The shortened URL for the Folder
  - `updated_at` - The updated at date for the Folder
  - `deleted_at` - The deletion date for the Folder
  - `folders` - A JSON Array of the folders within the Folder
  - `files` - A JSON Array of the files within the Folder

#### Creating a Folder

Creates a Folder at the given Folder path under the referenced Organization. Folder paths can be nested as many levels as is necessary.

 Definition

    POST /api/v1/:organization_name/:project/:folder_path

 Example Request

  ```shell
  $ curl -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations'
  ```

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

#### Deleting a Folder

Deletes the requested folder under the references Organization.

Definition

    DELETE /api/v1/:organization_name/:project/:folder_path

Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations'
```

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

```shell
$ curl -X POST \
    -d 'to=/NewTest/NewIllustrations' \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/move'
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
The :organization_name, :project and :folder_path are required in the call URL. A POST parameter of :to is required to specify the destination location under the referenced Organization that the folder is to be moved to.

#### Returns

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

#### Changing a Folder's color

Change a referenced Folder's color on LayerVault.com and the user's local file system.

Definition

    PUT /api/v1/:organization_name/:project/:folder_path

Example Request

```shell
$ curl -X PUT \
    -d "color=000000" \
    -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations'
```

Example Response

    None

#### Arguments

The :organization_name, :project and :folder_path are required in the call URL. A PUT parameter :color representing the Hex color value the folder should be set to is required.

#### Returns

- HTTP Response: 200 on success.
