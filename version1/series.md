# Series

* [Structure](#structure)
* [Embeds](#embeds)
* [GET /series](#get-series)
* [GET /series/{id}](#get-seriesid)
* [GET /series/{id}/games](#get-seriesidgames)

Series are collections of [games](games.md) that have been released in context to each other, for
example the "GTA" series contains all the games of this franchise. As a series is most often formed
*after* a number of games have been published, many games do not belong to a specific series and are
therefore categorized in the "Other" series. As time progresses, games can be moved in their own
series, so be prepared for the series-game relationship to change.

### Structure

Represented as JSON, a series looks like this:

```json
{
  "id": "1kgr75w4",
  "names": {
    "international": "Grand Theft Auto",
    "japanese": ""
  },
  "abbreviation": "gta",
  "weblink": "http://www.speedrun.com/gta",
  "moderators": {
    "wzx7q875": "moderator",
    "zzb12med": "super-moderator"
  },
  "created": "2014-12-07T12:50:20Z",
  "assets": {
    "logo": {
      "uri": "http://www.speedrun.com/themes/mk64/logo.png",
      "width": 180,
      "height": 34
    },
    "cover-tiny": {
      "uri": "http://www.speedrun.com/themes/mk64/cover-32.png",
      "width": 32,
      "height": 45
    },
    "cover-small": {
      "uri": "http://www.speedrun.com/themes/mk64/cover-64.png",
      "width": 64,
      "height": 90
    },
    "cover-medium": {
      "uri": "http://www.speedrun.com/themes/mk64/cover-128.png",
      "width": 128,
      "height": 180
    },
    "cover-large": {
      "uri": "http://www.speedrun.com/themes/mk64/cover-256.png",
      "width": 256,
      "height": 360
    },
    "icon": {
      "uri": "http://www.speedrun.com/themes/mario_kart/favicon.png",
      "width": 44,
      "height": 44
    },
    "trophy-1st": {
      "uri": "http://www.speedrun.com/images/icons/goldtrophy.png",
      "width": 16,
      "height": 16
    },
    "trophy-2nd": {
      "uri": "http://www.speedrun.com/images/icons/silvertrophy.png",
      "width": 16,
      "height": 16
    },
    "trophy-3rd": {
      "uri": "http://www.speedrun.com/images/icons/bronzetrophy.png",
      "width": 16,
      "height": 16
    },
    "trophy-4th": null,
    "background": {
      "uri": "http://www.speedrun.com/themes/mk64/background.png",
      "width": 151,
      "height": 195
    },
    "foreground": null
  },
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/series/wnlod5vz"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/series/wnlod5vz/games"
  }]
}
```

Things to note:

* ``id`` values can vary in length.
* ``created`` can be ``null`` for series that have been added in the early days of speedrun.com.
* ``weblink`` is the link to get to this series' page intended for humans, ``links`` are meant for
  the API client.
* **Do not** use the ``abbreviation`` to manually construct a series' full web URL, instead always
  use the provided ``weblink``. We might change the URL scheme on the frontend at any time without
  prior notice!
* The japanese title can be ``null``.
* ``moderators`` is a mapping of user IDs to their roles within the series. Possible roles are
  ``moderator`` and ``super-moderator`` (super moderators can appoint other users as moderators).
* ``assets`` are links to images that are used for that series on speedrun.com. Except for
  ``background``, ``foreground`` and ``trophy-4th``, all links are always given, even if they
  point to empty placeholder images.

  Note that there are a few series that have time-dependent styles, which are not reflected in the
  API. For those series, only the regular images are returned.

  ``width`` and ``height`` are pixel values.

### Embeds

You can [embed](embedding.md) the following resources into a series:

* ``moderators`` will embed the moderators as full user resources.

### GET /series

This will return a list of all series. You can filter the result by a few things:

Query Parameter  | Type   | Description
---------------- | ------ | ------------------------------------------------------------------
``name``         | string | when given, performs a fuzzy search across series names and abbreviations
``abbreviation`` | string | when given, performs an exact-match search for this abbreviation
``moderator``    | string | moderator ID; when given, only series moderated by that user will be returned

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by               | Description
---------------------- | ------------------------------------------------------------------
``name.int`` (default) | sorts alphanumerically by the international name
``name.jap``           | sorts alphanumerically by the japanese name
``abbreviation``       | sorts alphanumerically by the abbreviation
``created``            | sorts by the date when the series was added on speedrun.com

##### Example Requests

* [**GET /api/v1/series**](http://www.speedrun.com/api/v1/series) gets all series
* [**GET /api/v1/series?orderby=created&direction=desc**](http://www.speedrun.com/api/v1/series?orderby=created&direction=desc)
  gets all series, newest first.
* [**GET /api/v1/series?name=mario**](http://www.speedrun.com/api/v1/series?name=mario) searches for
  Mario series

##### Example Response

```json
{
  "data": [
    <series>,
    <series>,
    <series>,
    <series>,
    ...
  ]
}
```

### GET /series/{id}

This will retrieve a single series, identified by its ID. Instead of the series' ID, you can also
specify the series' abbreviation. When an abbreviation was found, the API will respond with a redirect
the the ID-based URL (so ``/api/v1/series/aoe`` will be redirected to ``/api/v1/series/yr4gy141``).

##### Example Requests

* [**GET /api/v1/series/rv7emz49**](http://www.speedrun.com/api/v1/series/rv7emz49) retrieves the
  Super Mario series.

##### Example Response

```json
{
  "data": <series>
}
```

### GET /series/{id}/games

This will retrieve all games (excluding romhacks) of a given series (the ID can be either the actual
series ID or its abbreviation). You can filter the result by the same attributes as you can filter
the [complete game list](games.md#get-games), except that you cannot use the ``romhack`` parameter[1].

You can also use the sorting options as for the complete game list.

As with the complete game list, bulk mode is available on this resource as well.

[1] Why? Because this resource is about representing the *hierarchy* between a series and the games
that belong to it; from a hierarchy standpoint, romhacks are children of games and so you need to
ask a game for its children (= romhacks). The complete game list, however, is all about getting a
flat list of playable things that have runs, which is why romhacks are included in there and can be
filtered by.

##### Example Requests

* [**GET /api/v1/series/rv7emz49/games**](http://www.speedrun.com/api/v1/series/rv7emz49/games)
  retrieves the games of the Super Mario series.
* [**GET /api/v1/series/rv7emz49/games?released=2013**](http://www.speedrun.com/api/v1/series/rv7emz49/games?released=2013)
  retrieves only those Mario games released in 2013.
* [**GET /api/v1/series/rv7emz49/games?_bulk=yes**](http://www.speedrun.com/api/v1/series/rv7emz49/games?_bulk=yes)
  retrieves the games of the Super Mario series, with less data being returned per game.

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
