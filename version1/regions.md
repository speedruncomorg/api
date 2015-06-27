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

This will return *all* regions.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by           | Description
------------------ | ------------------------------------------------------------------
``name`` (default) | sorts alphanumerically by the region name

(Yes, currently only the ``direction`` makes sense for regions. This is not a bug.)

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
