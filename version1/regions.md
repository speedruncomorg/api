# Regions

* [Structure](#structure)
* [GET /regions](#get-regions)
* [GET /regions/{id}](#get-regionsid)

Regions represent the different distribution zones in which [games](games.md) are published, for
example the US, Europe or Japan.

### Structure

Represented as JSON, a region looks like this:

```json
{
  "id": "pr184lqn",
  "name": "USA / NTSC",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/regions/pr184lqn"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/games?region=pr184lqn"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?region=pr184lqn"
  }]
}
```

### GET /regions

This will return *all* regions ordered by name.

### GET /regions/{id}

This will retrieve a single region, identified by its ID.

##### Example Requests

* [**GET /api/v1/regions/pr184lqn**](http://www.speedrun.com/api/v1/regions/pr184lqn) retrieves the
  "USA/NTSC" region.

##### Example Response

```json
{
  "data": <region>
}
```
