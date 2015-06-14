# Games

* [Structure](#structure)
* [GET /v1/games](#get-v1games)
* [GET /v1/games/{id}](#get-v1gamesid)
* [GET /v1/games/{id}/categories](#get-v1gamesidcategories)
* [GET /v1/games/{id}/levels](#get-v1gamesidlevels)
* [GET /v1/games/{id}/variables](#get-v1gamesidvariables)
* [GET /v1/games/{id}/children](#get-v1gamesidchildren)

Games are the things [users](users.md) do speedruns in. Games are associated with [regions](regions.md)
(US, Europe, ...), [platforms](platforms.md) (consoles, handhelds, ...), [categories](categories.md),
[levels](levels.md) and [custom variables](variables.md) (like speed=50/100/150cc in Mario Kart
games).

Games are organized in a series->game hierarchy. Some games have "parent games", like Super Mario
Sunshine has "Super Mario Series" as its parent. Series can have runs, too, that's why you will get
series just like regular games via the API. Besides this distinction, series and games are the same.

### Structure

Represented as JSON, a game looks like this:

```json
{
  "id": 1280,
  "names": {
    "international": "Super Mario Sunshine",
    "japanese": "\u30b9\u30fc\u30d1\u30fc\u30de\u30ea\u30aa\u30b5\u30f3\u30b7\u30e3\u30a4\u30f3"
  },
  "weblink": "http://www.speedrun.com/sms",
  "released": 2002,
  "ruleset": {
    "show-milliseconds": false,
    "require-verification": true,
    "require-video": false
  },
  "platforms": [4, 33],
  "regions": [1, 2, 3, 5],
  "moderators": {
    "123": "moderator",
    "456": "super-moderator"
  },
  "created": "2014-12-07T12:50:20Z",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/games/1280"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?game=1280"
  }, {
    "rel": "levels",
    "uri": "http://www.speedrun.com/api/v1/games/1280/levels"
  }, {
    "rel": "categories",
    "uri": "http://www.speedrun.com/api/v1/games/1280/categories"
  }, {
    "rel": "variables",
    "uri": "http://www.speedrun.com/api/v1/games/1280/variables"
  }, {
    "rel": "parent",
    "uri": "http://www.speedrun.com/api/v1/games/30"
  }, {
    "rel": "children",
    "uri": "http://www.speedrun.com/api/v1/games/1280/children"
  }]
}
```

Things to note:

* ``created`` can be ``null`` for games that have been added in the early days of speedrun.com.
* ``weblink`` is the link to get to this game's page intended for humans, ``links`` are meant for
  the API client.
* The japanese title can be ``null``.
* ``platforms`` is a list of platform IDs the game can be played on. This list can be empty.
* ``regions`` is a list of region IDs the game is available in. This list can be empty.
* ``moderators`` is a mapping of user IDs to their roles within the game. Possible roles are
  ``moderator`` and ``super-moderator`` (super moderators can appoint other users as moderators).

### Embeds

You can [embed](embedding.md) the following resources into a game:

* ``levels`` will embed all levels defined for the game.
* ``categories`` will embed *all* defined categories for the game.
* ``moderators`` will embed the moderators as full user resources.
* ``platforms`` will embed all assigned platforms.
* ``regions`` will embed all assigned regions.
* ``variables`` will embed *all* defined variables for the game.

### GET /v1/games

This will return a list of all games. You can filter the result by a few things:

Query Parameter  | Type   | Description
---------------- | ------ | ------------------------------------------------------------------
``name``         | string | when given, performs a substring search across game names and URLs
``released``     | int    | when given, restricts to games released in that year
``platform``     | int    | platform ID; when given, restricts to that platform
``region``       | int    | region ID; when given, restricts to that region
``moderator``    | int    | moderator ID; when given, only games moderated by that user will
                 |        | be returned

Note that giving invalid values for ``platform``, ``region`` or ``moderator`` will result in an
HTTP 404 error instead of an empty list. This is on purpose, because asking to filter by non-existing
elements should be something an API client should notice.

##### Example Requests

* [**GET /api/v1/games**](http://www.speedrun.com/api/v1/games) gets all games
* [**GET /api/v1/games?names=mario**](http://www.speedrun.com/api/v1/games?name=mario) searches for
  Mario games
* [**GET /api/v1/games?region=4&released=1999**](http://www.speedrun.com/api/v1/games?region=4&released=1999)
  searches for all games on the iQue (region 4) that have been released in 1999.

##### Example Response

```json
{
  "data": [
    <game>,
    <game>,
    <game>,
    <game>,
    ...
  ]
}
```

### GET /v1/games/{id}

This will retrieve a single game, identified by its ID.

##### Example Requests

* [**GET /api/v1/games/1280**](http://www.speedrun.com/api/v1/games/1280) retrieves Super Mario
  Sunshine.

##### Example Response

```json
{
  "data": <game>
}
```

### GET /v1/games/{id}/categories

This will retrieve *all* [categories](categories.md) of a given game. If you need only those
applicable to certain [levels](levels.md), look there.

##### Example Requests

* [**GET /api/v1/games/1280/categories**](http://www.speedrun.com/api/v1/games/1280/categories)
  retrieves the categories of Super Mario Sunshine.

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

### GET /v1/games/{id}/levels

This will retrieve *all* [levels](levels.md) of a given game.

##### Example Requests

* [**GET /api/v1/games/1280/levels**](http://www.speedrun.com/api/v1/games/1280/levels) retrieves
  the levels of Super Mario Sunshine.

##### Example Response

```json
{
  "data": [
    <level>,
    <level>,
    <level>,
    <level>,
    ...
  ]
}
```

### GET /v1/games/{id}/variables

This will retrieve *all* [variables](variables.md) of a given game. If you need only those applicable
to certain [categories](categories.md) or [levels](levels.md), look there.

##### Example Requests

* [**GET /api/v1/games/454/variables**](http://www.speedrun.com/api/v1/games/454/variables) retrieves
  all variables defined for Mario Kart 8.

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

### GET /v1/games/{id}/children

This will retrieve all child games of a given game. If a game has children, we actually call it
"series", but as mentioned above, they behave as games and are treated as games.

##### Example Requests

* [**GET /api/v1/games/51/children**](http://www.speedrun.com/api/v1/games/51/children) retrieves
  all Mario Kart games.

##### Example Response

```json
{
  "data": [
    <game>,
    <game>,
    <game>,
    <game>,
    ...
  ]
}
```
