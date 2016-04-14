# Genres

* [Structure](#structure)
* [GET /genres](#get-genres)
* [GET /genres/{id}](#get-genresid)

Genres are classifications for [games](games.md), for example Action, JRPG, Rogue-like etc.

### Structure

Represented as JSON, a genre looks like this:

```json
{
  "id": "qdnqyk28",
  "name": "Metroidvania",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/genres/qdnqyk28"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/games?genre=qdnqyk28"
  }]
}
```

### GET /genres

This will return *all* genres.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by           | Description
------------------ | ------------------------------------------------------------------
``name`` (default) | sorts alphanumerically by the genre name

(Yes, currently only the ``direction`` makes sense for genres. This is not a bug.)

### GET /genres/{id}

This will retrieve a single genre, identified by its ID.

##### Example Requests

* [**GET /api/v1/genres/qdnqyk28**](http://www.speedrun.com/api/v1/genres/qdnqyk28) retrieves
  Metroidvania.

##### Example Response

```json
{
  "data": <genre>
}
```
