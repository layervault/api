### Revisions

#### Retrieving Revisions' Information

Returns a referenced File Revision's information.

 Definition

    GET /api/v1/:organization_name/:project/:folder_path/:file_name/:revision

 Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervault/Test/Illustrations/NewFile.psd/1'
```

 Example Response

```json
{
  "name": "1",
  "download_url": "https://layervault.com/files/download_node/DwoRKvzbEj",
  "full_url": "https://layervault.com/layervault/api-playground/Test.psd/1",
  "md5": "e388875b2d81f6798dd187d1c047a50c",
  "deleted_at": null,
  "updated_at": "2013-10-21T19:05:24Z",
  "created_at": "2013-10-21T19:05:24Z",
  "shortened_url": "http://lyrv.lt/DwoRKvzbEj",
  "revision_number": 1,
  "user": {},
  "previews": []
}
```

#### Arguments
The :organization_name, :project, :folder_path, :file_name and :revision are required in the call URL.

#### Returns

Returns a JSON object containing:

  - `name` - The name of the revision, which is its revision number.
  - `full_url` - The absolute URL to the Revision
  - `download_url` - The absolute URL to download a copy of the Revision
  - `md5` - The MD5 hash of the Revision
  - `shortened_url` - The shortened URL for the Folder
  - `deleted_at` - The date when the revision was deleted, null otherwise
  - `updated_at` - The updated at date for the Revision
  - `deleted_at` - The deletion date for the Revision
  - `revision_number` - The revision number for the Revision
  - `user` - The user who created the Revision
  - `previews` - All previews generated for the Revision

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

#### Retreiving a File Revision's Feedback Items

Obtain the feeback items attached to a file revision.

Definition

    GET /api/v1/:organization_name/:project/:folder/:file_name/:revision/feedback_items

Example Request

```shell
$ curl -H 'Authorization: Bearer <your access token>' \
  'https://api.layervault.com/api/v1/layervault/Project/Test.psd/1/feedback_items'
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
