### Organization

#### Retrieving Organization Information

This call returns the organization information for the requested Organization.

 Definition

    GET /api/v1/:organization_name

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://api.layervault.com/api/v1/layervault'

 Example Response

```json
{
  "name":"LayerVault",
  "deleted_at":"3013-02-26T16:53:38Z",
  "updated_at":"2013-10-22T00:11:57Z",
  "full_url":"https://layervault.com/layervault",
  "sync_type":"layervault",
  "projects": [
    {
      "name":"api-playground",
      "deleted_at":"3013-01-01T00:00:00Z",
      "updated_at":"2013-10-21T19:05:40Z",_
      "color":null,
      "member": true
    }
  ]
}
```

#### Arguments
No arguments are required.

#### Returns

- HTTP Status: 200

Returns a JSON object containing:

  - `name` - The full name of the Organization.
  - `deleted_at` - The date the organization was deleted (Not deleted if `3013-02-26T16:53:38Z`)
  - `full_url` - The full URL to access the Organization.
  - `sync_type` - Indicates whether the Synchronization used is either ```layervault``` or ```dropbox```.
  - `projects` - An array of the projects that the user is able to see or are public for that Organization.
