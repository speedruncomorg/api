# Platforms

* [Structure](#structure)
* [GET /v1/platforms](#get-v1platforms)
* [GET /v1/platforms/{id}](#get-v1platformsid)

Platforms are hardware devices that run [games](games.md), for example PC, NES, PS2 etc.

### Structure

Represented as JSON, a platform looks like this:

```json
{
  "id": 1,
  "name": "Nintendo Entertainment System",
  "released": 2002,
  "links": [{
    "rel": "self",
    "uri": "/v1/platforms/1"
  }]
}
```

### GET /v1/platforms

This will return *all* platforms order by name.

### GET /v1/platforms/{id}

This will retrieve a single platform, identified by its ID.

##### Example Requests

* [**GET /api/v1/platforms/1**](http://www.speedrun.com/api/v1/platforms/1) retrieves the NES.

##### Example Response

```json
{
  "data": <platform>
}
```
