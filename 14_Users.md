### User Information

#### Retrieving User Information

This call returns the user information for which the Client is acting on behalf of, including the Projects the user has access to and Organizations they are a member of.

 Definition

    GET /api/v1/me

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://api.layervault.com/api/v1/me'

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
      "member": true,
      "organization_name": "Acme Co",
      "organization_permalink": "acme-co"
    }
  ],
  "organizations": [
    {
      "name": "Acme Co",
      "deleted_at": null,
      "updated_at": "2013-10-25T22:52:44Z",
      "full_url": "https://layervault.com/acme-co",
      "sync_type": "layervault",
      "projects": [
        {
          "name": "Folder",
          "deleted_at": null,
          "updated_at": "2013-10-25T22:52:44Z",
          "color": null,
          "member": true,
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

  - `email` - The email address of the user.
  - `first_name` - The First name of the user.
  - `last_name` - The Last name of the user.
  - `is_admin` - Whether the user is an admin of the site or not.
  - `projects` - An JSON array of the projects that the user is either a member or part of an organization where they are an observer.
  - `organizationa` - An JSON array of the organizations that the user is a member or observer of.