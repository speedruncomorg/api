# Games

* [Structure](#structure)
* [GET /v1/games](#get-v1games)
* [GET /v1/games/{id}](#get-v1gamesid)
* [GET /v1/games/{id}/categories](#get-v1gamesidcategories)
* [GET /v1/games/{id}/levels](#get-v1gamesidlevels)

Games are the things [users](#users) do speedruns in. Games are associated with regions (US, Europe,
...), platforms (consoles, handhelds, ...), categories, levels and custom variables
(like speed=50/100/150cc in Mario Kart games).

Games are organized in a series->game hierarchy. Some games have "parent games", like Super Mario
Sunshine has "Super Mario Series" as its parent. Series can have runs, too, that's why you will get
series just like regular games via the API. Besides this distinction, series and games are the same.

You can request to embed the following for each game: ``levels`` and ``categories``.

### Structure

Represented as JSON, a game looks like this:

```json
{
  "id": 1280,
  "names": {
    "international": "Super Mario Sunshine",
    "japanese": "\u30b9\u30fc\u30d1\u30fc\u30de\u30ea\u30aa\u30b5\u30f3\u30b7\u30e3\u30a4\u30f3"
  },
  "weblink": "/sms",
  "released": 2002,
  "ruleset": {
    "show-milliseconds": false,
    "require-verification": true,
    "require-video": false
  },
  "platforms": [4, 33],
  "regions": [1, 2, 3, 5],
  "created": "2014-12-07T12:50:20Z",
  "links": [{
    "rel": "self",
    "uri": "/v1/games/1280"
  }, {
    "rel": "runs",
    "uri": "/v1/runs?game=1280"
  }, {
    "rel": "levels",
    "uri": "/v1/games/1280/levels"
  }, {
    "rel": "categories",
    "uri": "/v1/games/1280/categories"
  }, {
    "rel": "parent",
    "uri": "/v1/games/30"
  }]
}
```

Things to note:

* ``created`` can be ``null`` for games that have been added in the early days of speedrun.com.
* ``weblink`` is the link to get to this game's page intended for humans, ``links`` are meant for
  the API client.
* The japanese title can be ``null``.

### GET /v1/games

This will return a list of N games, by default N=20. You can filter the result by a few things:

* ``?name=...`` will search for the given name.
* ``?released=...`` will only return games released in that year.

##### Example Requests

* [**GET /api/v1/games**](http://www.speedrun.com/api/v1/games) gets all games
* [**GET /api/v1/games?names=mario**](http://www.speedrun.com/api/v1/games?name=mario) searches for
  Mario games
* [**GET /api/v1/games?released=2012&name=auto**](http://www.speedrun.com/api/v1/games?released=2012&name=auto)
  searches all games released in 2012 with "auto" somewhere in their name

##### Example Response

```json
{
  "data": [
    { *game* },
    { *game* },
    { *game* },
    { *game* },
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
  "data": { *game* }
}
```

### GET /v1/games/{id}/categories

This will retrieve *all* categories of a given game. This includes only **full-game** categories, not
those used for individual-level runs.

##### Example Requests

* [**GET /api/v1/games/1280/categories**](http://www.speedrun.com/api/v1/games/1280/categories)
  retrieves the categories of Super Mario Sunshine.

##### Example Response

```json
{
  "data": [
    { *category* },
    { *category* },
    { *category* },
    { *category* },
    ...
  ]
}
```

### GET /v1/games/{id}/levels

This will retrieve *all* levels of a given game.

##### Example Requests

* [**GET /api/v1/games/1280/levels**](http://www.speedrun.com/api/v1/games/1280/levels) retrieves
  the levels of Super Mario Sunshine.

##### Example Response

```json
{
  "data": [
    { *level* },
    { *level* },
    { *level* },
    { *level* },
    ...
  ]
}
```
