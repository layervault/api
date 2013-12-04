### Revisions

#### Retrieving Revisions' Information

Returns a referenced File Revision's information.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision

 Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd/1'
```

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

  - `full_url` - The absolute URL to the File
  - `download_url` - The absolute URL to download a copy of the File
  - `local_path` - The local path on the user's filesystem
  - `md5` - The MD5 hash of the File
  - `shortened_url` - The shortened URL for the Folder
  - `updated_at` - The updated at date for the Folders
  - `deleted_at` - The deletion date for the Folders
  - `revision_number` - The revision number of the File

#### Retrieving Revision's Meta Information

Returns a referenced File Revisions Meta Information. Contains various metadata about the file format.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/meta

 Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
    'https://api.layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd/1/meta'
```

 Example Response

```json
{
  "psd": {
    "children": [
      {
        "type": "group",
        "visible": false,
        "opacity": 1.0,
        "blending_mode": "normal",
        "name": "design aids",
        "left": 220,
        "right": 2100,
        "top": 0,
        "bottom": 2640,
        "height": 1880,
        "width": 2640,
        "children": [
          {
            "type": "layer",
            "visible": true,
            "opacity": 0.2,
            "blending_mode": "normal",
            "name": "Layer 8",
            "left": 220,
            "right": 2100,
            "top": 1559,
            "bottom": 1759,
            "height": 200,
            "width": 1880,
            "text": null,
            "ref_x": 0.0,
            "ref_y": 1259.0
          },
          {
            "type": "layer",
            "visible": true,
            "opacity": 0.2,
            "blending_mode": "normal",
            "name": "Layer 1",
            "left": 220,
            "right": 2100,
            "top": 0,
            "bottom": 2640,
            "height": 2640,
            "width": 1880,
            "text": null,
            "ref_x": 200.0,
            "ref_y": 0.0
          }
        ]
      },
      {
        "type": "group",
        "visible": true,
        "opacity": 1.0,
        "blending_mode": "normal",
        "name": "header",
        "left": 0,
        "right": 2320,
        "top": -80,
        "bottom": 120,
        "height": 2320,
        "width": 200,
        "children": [
          {
            "type": "group",
            "visible": false,
            "opacity": 1.0,
            "blending_mode": "normal",
            "name": "logo",
            "left": 219,
            "right": 337,
            "top": 15,
            "bottom": 114,
            "height": 118,
            "width": 99,
            "children": [
              {
                "type": "layer",
                "visible": true,
                "opacity": 1.0,
                "blending_mode": "normal",
                "name": "DN",
                "left": 237,
                "right": 294,
                "top": 45,
                "bottom": 75,
                "height": 30,
                "width": 57,
                "text": {
                  "value": "DN",
                  "font": {
                    "name": "AvantGardeBQ-Bold",
                    "sizes": [
                      56.0
                    ],
                    "colors": [
                      [
                        255,
                        45,
                        114,
                        217
                      ]
                    ],
                    "css": "font-family: \"AvantGardeBQ-Bold\", \"AdobeInvisFont\", \"MyriadPro-Regular\";\nfont-size: 56.0pt;\ncolor: rgba(45, 114, 217, 255);"
                  },
                  "left": 0,
                  "top": 0,
                  "right": 0,
                  "bottom": 0,
                  "transform": {
                    "xx": 0.75,
                    "xy": 0.0,
                    "yx": 0.0,
                    "yy": 0.75,
                    "tx": 265.2851736545563,
                    "ty": 75.36329567432404
                  }
                },
                "ref_x": -1200.9999825954437,
                "ref_y": -314.62498557567596
              }
    ],
    "document": {
      "width": 2320,
      "height": 2640
    }
  }
  }
}
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revisions are required in the call URL.

#### Returns

Returns a JSON object containing any Meta information that was set for the revision.

#### Retrieving Revision's Preview Information

Returns a referenced File Revisions Preview image link.

 Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/preview

 Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervaultTest/Illustrations/NewFile.psd/1/previews?w=100&h=100'
```

 Example Response

```json
'https://layervault-preview.imgix.net/data/ed3fd8ba3ff2acf4157517fb14aadf8b?w=600&h=1012&s=6666868519b243260d38b3ca1d1b981f'
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revision are required in the call URL. In addition, the GET parameters :w for width and :h for height must be specified.

#### Returns

A single URL to the preview image.