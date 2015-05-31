# Categories

* [Structure](#structure)
* [GET /v1/categories/{id}](#get-v1categoriesid)

Categories are the different rulesets for speedruns. Categories are either ``per-game`` or ``per-level``
(if the game uses individual levels), both can be accessed via this resource.

This resource is meant to perform ID-based lookups. That's why there is no ``GET /categories`` to
get a flat list of all categories across all games. If you want to get the categories for a game,
use ``GET /v1/games/{gameid}/categories``.

### Structure

Represented as JSON, a category looks like this:

```json
{
  "id": 1,
  "name": "Any%",
  "type": "per-game",
  "rules": "Yada Yada Yada",
  "players": 1,
  "miscellaneous": false,
  links: [{
    rel: "self",
    uri: "/v1/categories/1"
  }, {
    rel: "game",
    uri: "/v1/games/19"
  }]
}
```

### GET /v1/categories/{id}

This will retrieve a single category, identified by its ID.

##### Example Requests

* [**GET /api/v1/categories/1**](http://www.speedrun.com/api/v1/categories/1) retrieves Any% of
  GTA Vice City.

##### Example Response

```json
{
  "data": { *category* }
}
```
