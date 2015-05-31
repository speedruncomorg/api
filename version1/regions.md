# Regions

* [Structure](#structure)
* [GET /v1/regions](#get-v1regions)
* [GET /v1/regions/{id}](#get-v1regionsid)

Regions represent the different distribution zones in that games are published, for example the US,
Europe or Japan.

### Structure

Represented as JSON, a region looks like this:

```json
{
  "id": 1,
  "name": "USA / NTSC",
  "links": [{
    "rel": "self",
    "uri": "/v1/regions/1"
  }]
}
```

### GET /v1/regions

This will return *all* regions order by name.

### GET /v1/regions/{id}

This will retrieve a single region, identified by its ID.

##### Example Requests

* [**GET /api/v1/regions/1**](http://www.speedrun.com/api/v1/regions/1) retrieves the "USA/NTSC"
  region.

##### Example Response

```json
{
  "data": <region>
}
```
