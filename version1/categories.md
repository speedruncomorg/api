# Categories

* [Structure](#structure)
* [Embeds](#embeds)
* [GET /categories/{id}](#get-categoriesid)
* [GET /categories/{id}/variables](#get-categoriesidvariables)

Categories are the different rulesets for speedruns. Categories are either ``per-game`` or ``per-level``
(if the game uses individual levels), both can be accessed via this resource.

This resource is meant to perform ID-based lookups. That's why there is no ``GET /categories`` to
get a flat list of all categories across all games. If you want to get the categories for a game,
use ``GET /v1/games/{gameid}/categories``.

### Structure

Represented as JSON, a category looks like this:

```json
{
  "id": "jkh473tf",
  "name": "Any%",
  "weblink": "http://www.speedrun.com/tww#Anypc",
  "type": "per-game",
  "rules": "Yada Yada Yada",
  "players": {
    "type": "exactly",
    "value": 2
  },
  "miscellaneous": false,
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/categories/jkh473tf"
  }, {
    "rel": "game",
    "uri": "http://www.speedrun.com/api/v1/games/i2grf78a"
  }, {
    "rel": "variables",
    "uri": "http://www.speedrun.com/api/v1/games/i2grf78a/variables"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?category=jkh473tf"
  }]
}
```

* ``id`` values can vary in length.
* As mentioned earlier, ``type`` can be ``per-game`` or ``per-level``.
* ``rules`` is a freeform text with some basic, undocumented speedrun.com markup.
* ``players`` is the number of participants for runs in this category. At the moment, there are
  two possible ``type`` values: ``exactly`` and ``up-to``, which should be self-explanatory.
* ``miscellaneous`` categories are usually not shown directly on the leaderboards, but are otherwise
  nothing special.
* The ``weblink`` is the URL to the category leaderboard on the website. Note that for ``per-level``
  categories, the ``weblink`` only points to the game page, because the link depends on the chosen
  level. *However*, when fetching categories in the context of a level (e.g. by requesting
  ``/api/v1/levels/<leve id>/categories``), the ``weblink`` will be set to the category leaderboard
  for that level.

### Embeds

You can [embed](embedding.md) the following resources into a category:

* ``game`` will include the game the category belongs to.
* ``variables`` will embed the applicable variables for this category. See below for another way
  to get these and what applicable means.

### GET /categories/{id}

This will retrieve a single category, identified by its ID.

##### Example Requests

* [**GET /api/v1/categories/nxd1rk8q**](http://www.speedrun.com/api/v1/categories/nxd1rk8q) retrieves
  Any% of GTA Vice City.

##### Example Response

```json
{
  "data": <category>
}
```

### GET /categories/{id}/variables

This will retrieve all [variables](variables.md) that are *applicable* to the given category.

* ``per-level`` categories will not show the variables for runs completing the entire game.
* ``per-game`` categories will not show the variables for individual level runs.

To get *all* variables defined for a game, see the [games](games.md) documentation.

##### Example Requests

* [**GET /api/v1/categories/xd1m7rd8/variables**](http://www.speedrun.com/api/v1/categories/xd1m7rd8/variables)
  retrieves the variables for Mario Kart 8's "32 Tracks" category.

##### Example Response

```json
{
  "data": [
    <variable>,
    <variable>,
    <variable>,
    ...
  ]
}
```
