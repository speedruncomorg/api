# Publishers

* [Structure](#structure)
* [GET /publishers](#get-publishers)
* [GET /publishers/{id}](#get-publishersid)

Publishers are companies that publish [games](games.md), for example Capcom, SEGA, Midway Games etc.

### Structure

Represented as JSON, a publisher looks like this:

```json
{
  "id": "1z6qgr9p",
  "name": "Codemasters",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/publishers/1z6qgr9p"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/games?publisher=1z6qgr9p"
  }]
}
```

### GET /publishers

This will return *all* publishers.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by           | Description
------------------ | ------------------------------------------------------------------
``name`` (default) | sorts alphanumerically by the publisher name

(Yes, currently only the ``direction`` makes sense for publishers. This is not a bug.)

### GET /publishers/{id}

This will retrieve a single publisher, identified by its ID.

##### Example Requests

* [**GET /api/v1/publishers/1z6qgr9p**](http://www.speedrun.com/api/v1/publishers/1z6qgr9p) retrieves
  Codemasters.

##### Example Response

```json
{
  "data": <publisher>
}
```
