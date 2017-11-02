# Engines

* [Structure](#structure)
* [GET /engines](#get-engines)
* [GET /engines/{id}](#get-enginesid)

Engines are software frameworks used in the creation of [games](games.md), for example the DOOM engine, Unreal Engine, Geo-Mod etc.

### Structure

Represented as JSON, an engine looks like this:

```json
{
  "id": "p85eo036",
  "name": "Build",
  "links": [{
    "rel": "self",
    "uri": "https://www.speedrun.com/api/v1/engines/p85eo036"
  }, {
    "rel": "games",
    "uri": "https://www.speedrun.com/api/v1/games?engine=p85eo036"
  }]
}
```

### GET /engines

This will return *all* engines.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by           | Description
------------------ | ------------------------------------------------------------------
``name`` (default) | sorts alphanumerically by the engine name

(Yes, currently only the ``direction`` makes sense for engines. This is not a bug.)

### GET /engines/{id}

This will retrieve a single engine, identified by its ID.

##### Example Requests

* [**GET /api/v1/engines/p85eo036**](https://www.speedrun.com/api/v1/engines/p85eo036) retrieves
  the Build engine.

##### Example Response

```json
{
  "data": <engine>
}
```
