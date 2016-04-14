# Game types

* [Structure](#structure)
* [GET /gametypes](#get-gametypes)
* [GET /gametypes/{id}](#get-gametypesid)

Game types are classifications for unofficial [games](games.md), for example ROM Hack, Fangame, Modification etc.

### Structure

Represented as JSON, a game type looks like this:

```json
{
  "id": "d91jd1ex",
  "name": "Fangame",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/gametypes/d91jd1ex"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/games?gametype=d91jd1ex"
  }]
}
```

### GET /gametypes

This will return *all* game types.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by           | Description
------------------ | ------------------------------------------------------------------
``name`` (default) | sorts alphanumerically by the game type name

(Yes, currently only the ``direction`` makes sense for game types. This is not a bug.)

### GET /gametypes/{id}

This will retrieve a single game type, identified by its ID.

##### Example Requests

* [**GET /api/v1/gametypes/d91jd1ex**](http://www.speedrun.com/api/v1/gametypes/d91jd1ex) retrieves
  Fangame.

##### Example Response

```json
{
  "data": <gametype>
}
```
