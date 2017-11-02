# Platforms

* [Structure](#structure)
* [GET /platforms](#get-platforms)
* [GET /platforms/{id}](#get-platformsid)

Platforms are hardware devices that run [games](games.md), for example PC, NES, PS2 etc.

### Structure

Represented as JSON, a platform looks like this:

```json
{
  "id": "rdjq4vwe",
  "name": "Nintendo Entertainment System",
  "released": 2002,
  "links": [{
    "rel": "self",
    "uri": "https://www.speedrun.com/api/v1/platforms/rdjq4vwe"
  }, {
    "rel": "games",
    "uri": "https://www.speedrun.com/api/v1/games?platform=rdjq4vwe"
  }, {
    "rel": "runs",
    "uri": "https://www.speedrun.com/api/v1/runs?platform=rdjq4vwe"
  }]
}
```

### GET /platforms

This will return *all* platforms.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by           | Description
------------------ | ------------------------------------------------------------------
``name`` (default) | sorts alphanumerically by the platform name
``released``       | sorts by the year the platform was released

### GET /platforms/{id}

This will retrieve a single platform, identified by its ID.

##### Example Requests

* [**GET /api/v1/platforms/rdjq4vwe**](https://www.speedrun.com/api/v1/platforms/rdjq4vwe) retrieves
  the NES.

##### Example Response

```json
{
  "data": <platform>
}
```
