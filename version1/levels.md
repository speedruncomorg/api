# Levels

* [Structure](#structure)
* [Embeds](#embeds)
* [GET /levels/{id}](#get-levelsid)
* [GET /levels/{id}/categories](#get-levelsidcategories)
* [GET /levels/{id}/variables](#get-levelsidvariables)
* [GET /levels/{id}/records](#get-levelsidrecords)

Levels are the stages/worlds/maps within a game. Not all [games](games.md) have levels.

This resource is meant to perform ID-based lookups. That's why there is no ``GET /levels`` to
get a flat list of all levels across all games. To only get the levels for one game, use the
``/levels`` collection on the game itself.

### Structure

Represented as JSON, a level looks like this:

```json
{
  "id": "ha71kldd",
  "name": "Slip Slide Icecapades",
  "weblink": "http://www.speedrun.com/twinsanity/Slip_Slide_Icecapades",
  "rules": "Yada Yada Yada",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/levels/ha71kldd"
  }, {
    "rel": "game",
    "uri": "http://www.speedrun.com/api/v1/games/sjkdzets"
  }, {
    "rel": "categories",
    "uri": "http://www.speedrun.com/api/v1/levels/ha71kldd/categories"
  }, {
    "rel": "variables",
    "uri": "http://www.speedrun.com/api/v1/levels/ha71kldd/variables"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?level=ha71kldd"
  }]
}
```

### Embeds

You can [embed](embedding.md) the following resources into a level:

* ``categories`` will embed the ``per-level`` [categories](categories.md) applicable for the requested
  level.
* ``variables`` will embed the [variables](variables.md) applicable for the requested level.

### GET /levels/{id}

This will retrieve a single level, identified by its ID.

##### Example Requests

* [**GET /api/v1/levels/329vpn9v**](http://www.speedrun.com/api/v1/levels/329vpn9v) retrieves "Slip
  Slide Icecapades" of Crash Twinsanity.

##### Example Response

```json
{
  "data": <level>
}
```

### GET /levels/{id}/categories

This will retrieve the *applicable* [categories](categories.md) for the given level. You can filter
the result by a few things:

Query Parameter   | Type   | Description
----------------- | ------ | -----------------------------------------
``miscellaneous`` | bool   | when given, filters (out) misc categories

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by          | Description
----------------- | ------------------------------------------------------------------
``name``          | sorts alphanumerically by the category name
``miscellaneous`` | sorts by miscellaneous flag
``pos`` (default) | uses the order as defined by the game moderator

##### Example Requests

* [**GET /api/v1/levels/329vpn9v/categories**](http://www.speedrun.com/api/v1/levels/329vpn9v/categories)
  retrieves the categories of the "Slip Slide Icecapades" level of Crash Twinsanity.
* [**GET /api/v1/levels/329vpn9v/categories?miscellaneous=no**](http://www.speedrun.com/api/v1/levels/329vpn9v/categories?miscellaneous=no)
  retrieves only the primary categories of the "Slip Slide Icecapades" level of Crash Twinsanity.

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

### GET /levels/{id}/variables

This will retrieve the *applicable* [variables](variables.md) for the given level.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by          | Description
----------------- | ------------------------------------------------------------------
``name``          | sorts alphanumerically by the variable name
``mandatory``     | sorts by mandatory flag
``user-defined``  | sorts by user-defined flag
``pos`` (default) | uses the order as defined by the game moderator

##### Example Requests

* [**GET /api/v1/levels/495ggmwp/variables**](http://www.speedrun.com/api/v1/levels/495ggmwp/variables)
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

### GET /levels/{id}/records

This will retrieve the records (first three places) of the given level for all available categories.

The following options are available in the query string:

Query Parameter  | Type   | Description
---------------- | ------ | ------------------------------------------------------------------
``top``          | int    | only return the top N *places* (this can result in more than N runs!); this is set to 3 by default
``skip-empty``   | bool   | when set to a true value, empty leaderboards will not show up in the result

This resource is [paginated](pagination.md).

The regular [leaderboard embeds](leaderboards.md#embeds) are available here as well.

##### Example Requests

* [**GET /api/v1/levels/rdnyx79m/records**](http://www.speedrun.com/api/v1/levels/rdnyx79m/records)
  retrieves the leaderboards for Super Mario World's "Yoshi's Island 3" level in all categories.

##### Example Response

```json
{
  "data": [
    <leaderboard>,
    <leaderboard>,
    <leaderboard>,
    ...
  ]
}
```
