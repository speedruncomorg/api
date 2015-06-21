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
    "uri": "http://www.speedrun.com/api/v1/platforms/rdjq4vwe"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/games?platform=rdjq4vwe"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?platform=rdjq4vwe"
  }]
}
```

### GET /platforms

This will return *all* platforms order by name.

### GET /platforms/{id}

This will retrieve a single platform, identified by its ID.

##### Example Requests

* [**GET /api/v1/platforms/rdjq4vwe**](http://www.speedrun.com/api/v1/platforms/rdjq4vwe) retrieves
  the NES.

##### Example Response

```json
{
  "data": <platform>
}
```
