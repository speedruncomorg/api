# Regions

* [Structure](#structure)
* [GET /regions](#get-regions)
* [GET /regions/{id}](#get-regionsid)

Regions represent the different distribution zones in that [games](games.md) are published, for
example the US, Europe or Japan.

### Structure

Represented as JSON, a region looks like this:

```json
{
  "id": 1,
  "name": "USA / NTSC",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/regions/1"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/games?region=1"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?region=1"
  }]
}
```

### GET /regions

This will return *all* regions order by name.

### GET /regions/{id}

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
