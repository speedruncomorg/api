# Levels

* [Structure](#structure)
* [GET /v1/levels/{id}](#get-v1levelsid)

Levels are the stages/worlds/maps within a game. Not all games have levels.

This resource is meant to perform ID-based lookups. That's why there is no ``GET /levels`` to
get a flat list of all levels across all games. If you want to get the levels for a game,
use ``GET /v1/games/{gameid}/levels``.

### Structure

Represented as JSON, a level looks like this:

```json
{
  "id": 420,
  "name": "Slip Slide Icecapades",
  "weblink: "/twinsanity/Slip_Slide_Icecapades",
  "rules": "Yada Yada Yada",
  "links": [{
    "rel": "self",
    "uri": "/v1/levels/420"
  }, {
    "rel": "game",
    "uri": "/v1/games/122"
  }, {
    "rel": "runs",
    "uri": "/v1/runs?game=122&level=420"
  }]
}
```

### GET /v1/levels/{id}

This will retrieve a single level, identified by its ID.

##### Example Requests

* [**GET /api/v1/levels/420**](http://www.speedrun.com/api/v1/levels/420) retrieves "Slip Slide
  Icecapades" of Crash Twinsanity.

##### Example Response

```json
{
  "data": { *level* }
}
```
