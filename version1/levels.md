# Levels

* [Structure](#structure)
* [Embeds](#embeds)
* [GET /v1/levels/{id}](#get-v1levelsid)
* [GET /v1/levels/{id}/categories](#get-v1levelsidcategories)
* [GET /v1/levels/{id}/variables](#get-v1levelsidvariables)

Levels are the stages/worlds/maps within a game. Not all [games](games.md) have levels.

This resource is meant to perform ID-based lookups. That's why there is no ``GET /levels`` to
get a flat list of all levels across all games. To only get the levels for one game, use the
``/levels`` collection on the game itself.

### Structure

Represented as JSON, a level looks like this:

```json
{
  "id": 420,
  "name": "Slip Slide Icecapades",
  "weblink": "http://www.speedrun.com/twinsanity/Slip_Slide_Icecapades",
  "rules": "Yada Yada Yada",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/levels/420"
  }, {
    "rel": "game",
    "uri": "http://www.speedrun.com/api/v1/games/122"
  }, {
    "rel": "categories",
    "uri": "http://www.speedrun.com/api/v1/levels/420/categories"
  }, {
    "rel": "variables",
    "uri": "http://www.speedrun.com/api/v1/levels/420/variables"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?level=420"
  }]
}
```

### Embeds

You can [embed](embedding.md) the following resources into a level:

* ``categories`` will embed the ``per-level`` [categories](categories.md) applicable for the requested
  level.
* ``variables`` will embed the [variables](variables.md) applicable for the requested level.

### GET /v1/levels/{id}

This will retrieve a single level, identified by its ID.

##### Example Requests

* [**GET /api/v1/levels/420**](http://www.speedrun.com/api/v1/levels/420) retrieves "Slip Slide
  Icecapades" of Crash Twinsanity.

##### Example Response

```json
{
  "data": <level>
}
```

### GET /v1/levels/{id}/categories

This will retrieve the *applicable* [categories](categories.md) for the given level.

##### Example Requests

* [**GET /api/v1/levels/420/categories**](http://www.speedrun.com/api/v1/levels/420/categories)
  retrieves the categories of the "Slip Slide Icecapades" level of Crash Twinsanity.

##### Example Response

```json
{
  "data": [
    <category>,
    <category>,
    <category>,
    <category>,
    ...
  ]
}
```

### GET /v1/levels/{id}/variables

This will retrieve the *applicable* [variables](variables.md) for the given level.

##### Example Requests

* [**GET /api/v1/levels/9610/variables**](http://www.speedrun.com/api/v1/levels/9610/variables)
  retrieves the variables applicable for the "Shrub Forest" level in Pokemon Rumble World.

##### Example Response

```json
{
  "data": [
    <variable>,
    <variable>,
    <variable>,
    <variable>,
    ...
  ]
}
```
