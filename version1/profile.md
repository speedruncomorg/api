# Profile

* [Structure](#structure)
* [GET /v1/profile](#get-v1profile)

The profile is the user resource of the currently [authenticated](../authentication.md) user. This
is useful to see what user a given API key belongs to.

**Note:** Accessing this obviously requires a valid API key.

### Structure

The profile is identical to the structure of [users](users.md), except that the ``self`` link points
to the profile and there is an additional ``user`` link that points to the user resource.

### GET /v1/profile

This will retrieve the current user.

##### Example Requests

* **GET /api/v1/profile**

##### Example Response

```json
{
  "data": <profile>
}
```
