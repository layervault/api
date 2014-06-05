### Public Projects

Public Projects are any project in LayerVault with its privacy set to "public."
These projects and their contents are visible to the world at large, without
needing to be a member of its containing organization.

Public Projects are exposed via a single endpoint and sorted in reverse-chronological
order from when they are created. For inspecting a project's hierarchy, please
see the other sections of the documentation.

30 public projects are returned per page. The indexing of pages is 1-based.

#### Retrieving a list of public projects.

 Definition

    GET /api/v1/public_projects

 Example Request

    $ curl -H 'Authorization: Bearer <your access token>' 'https://api.layervault.com/api/v1/public_projects'

 Example Request with Pagination

    $ curl -H 'Authorization: Bearer <your access token>' 'https://api.layervault.com/api/v1/public_projects?page=2'

 Example Response

```json
{
  "public_projects": [
    {
      "name": "Kelly Sutton's Project",
      "deleted_at": null,
      "updated_at": "2014-06-04T23:49:34Z",
      "color": "white",
      "slug": "kelly-sutton-s-project",
      "organization_name": "Kelly Sutton",
      "organization_permalink": "kelly-sutton"
    },
    ...
  ]
}
```