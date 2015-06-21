# Games

* [Structure](#structure)
* [Bulk Access](#bulkaccess)
* [Embeds](#embeds)
* [GET /games](#get-games)
* [GET /games/{id}](#get-gamesid)
* [GET /games/{id}/categories](#get-gamesidcategories)
* [GET /games/{id}/levels](#get-gamesidlevels)
* [GET /games/{id}/variables](#get-gamesidvariables)
* [GET /games/{id}/children](#get-gamesidchildren)

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
  "id": "1kgr75w4",
  "names": {
    "international": "Super Mario Sunshine",
    "japanese": "\u30b9\u30fc\u30d1\u30fc\u30de\u30ea\u30aa\u30b5\u30f3\u30b7\u30e3\u30a4\u30f3"
  },
  "abbreviation": "sms",
  "weblink": "http://www.speedrun.com/sms",
  "released": 2002,
  "ruleset": {
    "show-milliseconds": false,
    "require-verification": true,
    "require-video": false
  },
  "platforms": ["039or978", "hdzate15"],
  "regions": ["hdz48alk", "hdz1hdwo", "9uhjgcgg"],
  "moderators": {
    "wzx7q875": "moderator",
    "zzb12med": "super-moderator"
  },
  "created": "2014-12-07T12:50:20Z",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/games/1kgr75w4"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?game=1kgr75w4"
  }, {
    "rel": "levels",
    "uri": "http://www.speedrun.com/api/v1/games/1kgr75w4/levels"
  }, {
    "rel": "categories",
    "uri": "http://www.speedrun.com/api/v1/games/1kgr75w4/categories"
  }, {
    "rel": "variables",
    "uri": "http://www.speedrun.com/api/v1/games/1kgr75w4/variables"
  }, {
    "rel": "parent",
    "uri": "http://www.speedrun.com/api/v1/games/lkcbez17"
  }, {
    "rel": "children",
    "uri": "http://www.speedrun.com/api/v1/games/1kgr75w4/children"
  }]
}
```

Things to note:

* ``id`` values can vary in length.
* ``created`` can be ``null`` for games that have been added in the early days of speedrun.com.
* ``weblink`` is the link to get to this game's page intended for humans, ``links`` are meant for
  the API client.
* **Do not** use the ``abbreviation`` to manually construct a game's full web URL, instead always
  use the provided ``weblink``. We might change the URL scheme on the frontend at any time without
  prior notice!
* The japanese title can be ``null``.
* ``platforms`` is a list of platform IDs the game can be played on. This list can be empty.
* ``regions`` is a list of region IDs the game is available in. This list can be empty.
* ``moderators`` is a mapping of user IDs to their roles within the game. Possible roles are
  ``moderator`` and ``super-moderator`` (super moderators can appoint other users as moderators).

### Bulk Access

To make creating a list of all available games easier, games can be requested in a "bulk mode". This
will

* increase the maximum allowed [elements per page](pagination.md) to 1000,
* disable [embedding](embedding.md) of related resources and
* return a smaller representation for each game, described below.

Games fetched in bulk mode look like this:

```json
{
  "id": "1kgr75w4",
  "names": {
    "international": "Super Mario Sunshine",
    "japanese": "\u30b9\u30fc\u30d1\u30fc\u30de\u30ea\u30aa\u30b5\u30f3\u30b7\u30e3\u30a4\u30f3"
  },
  "abbreviation": "sms",
  "weblink": "http://www.speedrun.com/sms"
}
```

To enable bulk access, use the query string parameter ``_bulk`` and set it to a true value (1, yes
or true), e.g. ``GET /games?_bulk=yes&max=1000``. This only works for the games collection, not
individual games (so ``GET /games/1kgr75w4?_bulk=yes`` will ignore the ``_bulk`` parameter).

### Embeds

You can [embed](embedding.md) the following resources into a game:

* ``levels`` will embed all levels defined for the game.
* ``categories`` will embed *all* defined categories for the game.
* ``moderators`` will embed the moderators as full user resources.
* ``platforms`` will embed all assigned platforms.
* ``regions`` will embed all assigned regions.
* ``variables`` will embed *all* defined variables for the game.

Embedding is disabled in bulk mode.

### GET /games

This will return a list of all games. You can filter the result by a few things:

Query Parameter  | Type   | Description
---------------- | ------ | ------------------------------------------------------------------
``name``         | string | when given, performs a fuzzy search across game names and abbreviations
``abbreviation`` | string | when given, performs an exact-match search for this abbreviation
``released``     | int    | when given, restricts to games released in that year
``platform``     | string | platform ID; when given, restricts to that platform
``region``       | string | region ID; when given, restricts to that region
``moderator``    | string | moderator ID; when given, only games moderated by that user will be returned
``_bulk``        | bool   | enable [bulk access](#bulkaccess)

Note that giving invalid values for ``platform``, ``region`` or ``moderator`` will result in an
HTTP 404 error instead of an empty list. This is on purpose, because asking to filter by non-existing
elements should be something an API client should notice.

##### Example Requests

* [**GET /api/v1/games**](http://www.speedrun.com/api/v1/games) gets all games
* [**GET /api/v1/games?_bulk=yes&max=1000**](http://www.speedrun.com/api/v1/games?_bulk=yes&max=1000)
  gets all games with their smaller JSON representations
* [**GET /api/v1/games?names=mario**](http://www.speedrun.com/api/v1/games?name=mario) searches for
  Mario games
* [**GET /api/v1/games?region=mol4z19n&released=1999**](http://www.speedrun.com/api/v1/games?region=mol4z19n&released=1999)
  searches for all games on the iQue (region ``mol4z19n``) that have been released in 1999.

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

### GET /games/{id}

This will retrieve a single game, identified by its ID.

##### Example Requests

* [**GET /api/v1/games/v1pxjz68**](http://www.speedrun.com/api/v1/games/v1pxjz68) retrieves Super
  Mario Sunshine.

##### Example Response

```json
{
  "data": <game>
}
```

### GET /games/{id}/categories

This will retrieve *all* [categories](categories.md) of a given game. If you need only those
applicable to certain [levels](levels.md), look there. You can filter the result by a few things:

Query Parameter   | Type   | Description
----------------- | ------ | -----------------------------------------
``miscellaneous`` | bool   | when given, filters (out) misc categories

##### Example Requests

* [**GET /api/v1/games/v1pxjz68/categories**](http://www.speedrun.com/api/v1/games/v1pxjz68/categories)
  retrieves the categories of Super Mario Sunshine.
* [**GET /api/v1/games/v1pxjz68/categories?miscellaneous=no**](http://www.speedrun.com/api/v1/games/v1pxjz68/categories?miscellaneous=no)
  retrieves only the primary categories of Super Mario Sunshine.

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

### GET /games/{id}/levels

This will retrieve *all* [levels](levels.md) of a given game.

##### Example Requests

* [**GET /api/v1/games/v1pxjz68/levels**](http://www.speedrun.com/api/v1/games/v1pxjz68/levels) retrieves
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

### GET /games/{id}/variables

This will retrieve *all* [variables](variables.md) of a given game. If you need only those applicable
to certain [categories](categories.md) or [levels](levels.md), look there.

##### Example Requests

* [**GET /api/v1/games/kyd4pxde/variables**](http://www.speedrun.com/api/v1/games/kyd4pxde/variables)
  retrieves all variables defined for Mario Kart 8.

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

### GET /games/{id}/children

This will retrieve all child games of a given game. If a game has children, we actually call it
"series", but as mentioned above, they behave as games and are treated as games.

##### Example Requests

* [**GET /api/v1/games/n268w96p/children**](http://www.speedrun.com/api/v1/games/n268w96p/children)
  retrieves all Mario Kart games.

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
