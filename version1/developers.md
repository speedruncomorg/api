# Developers

* [Structure](#structure)
* [GET /developers](#get-developers)
* [GET /developers/{id}](#get-developersid)

Developers are the persons and/of studios responsible for developing [games](games.md), for example Acclaim Entertainment, Bethesda Softworks, Edmund McMillen etc.

### Structure

Represented as JSON, a developer looks like this:

```json
{
  "id": "l4eprzro",
  "name": "Arkane Studios",
  "links": [{
    "rel": "self",
    "uri": "https://www.speedrun.com/api/v1/developers/l4eprzro"
  }, {
    "rel": "games",
    "uri": "https://www.speedrun.com/api/v1/games?developer=l4eprzro"
  }]
}
```

### GET /developers

This will return *all* developers.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by           | Description
------------------ | ------------------------------------------------------------------
``name`` (default) | sorts alphanumerically by the developer name

(Yes, currently only the ``direction`` makes sense for developers. This is not a bug.)

### GET /developers/{id}

This will retrieve a single developer, identified by its ID.

##### Example Requests

* [**GET /api/v1/developers/l4eprzro**](https://www.speedrun.com/api/v1/developers/l4eprzro) retrieves
  Arkane Studios.

##### Example Response

```json
{
  "data": <developer>
}
```
