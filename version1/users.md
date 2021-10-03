# Users

* [Structure](#structure)
* [GET /users](#get-users)
* [GET /users/{id}](#get-usersid)
* [GET /users/{id}/personal-bests](#get-usersidpersonal-bests)

Users are the individuals who have registered an account on speedrun.com. Users submit (their)
[runs](runs.md) and moderate [games](games.md), besides other things that are not covered by this
API (like writing posts in the forums).

The API only returns public user information, so things like the e-mail address, site settings etc.
are not accessible.

### Structure

Represented as JSON, a single user looks like this:

```json
{
  "id": "kjpdr4jq",
  "names": {
    "international": "chewdiggy",
    "japanese": null
  },
  "pronouns": null,
  "weblink": "https://www.speedrun.com/user/chewdiggy",
  "name-style": {
    "style": "solid",
    "color": {
      "light": "#249BCE",
      "dark": "#44BBEE"
    }
  },
  "role": "user",
  "signup": "2014-10-02T12:34:23Z",
  "location": {
    "country": {
      "code": "GB",
      "names": {
        "international": "United Kingdom",
        "japanese": "\u30a4\u30ae\u30ea\u30b9"
      }
    },
    "region": {
      "code": "ENGLAND",
      "names": {
        "international": "England",
        "japanese": "\u30a4\u30f3\u30b0\u30e9\u30f3\u30c9"
      }
    }
  },
  "twitch": {
    "uri": "https://www.twitch.tv/username"
  },
  "hitbox": {
    "uri": "https://www.hitbox.tv/username"
  },
  "youtube": {
    "uri": "https://www.youtube.com/oih2grfwezf782zroufiw"
  },
  "twitter": {
    "uri": "https://www.twitter.com/username"
  },
  "speedrunslive": {
    "uri": "http://www.speedrunslive.com/profiles/#!/username"
  },
  "assets": {
    "icon": {
      "uri": null
    },
    "image": {
      "uri": "https://www.speedrun.com/userasset/48gr0rxp/image?v=cbb14df"
    }
  },
  "links": [{
    "rel": "self",
    "uri": "https://www.speedrun.com/api/v1/users/kjpdr4jq"
  }, {
    "rel": "runs",
    "uri": "https://www.speedrun.com/api/v1/runs?user=kjpdr4jq"
  }, {
    "rel": "games",
    "uri": "https://www.speedrun.com/api/v1/games?moderator=kjpdr4jq"
  }, {
    "rel": "personal-bests",
    "uri": "https://www.speedrun.com/api/v1/users/kjpdr4jq/personal-bests"
  }]
}
```

There are quite a few things that need a couple words of explanation.

* Most users don't have a ``japanese`` name.

* ``pronouns`` can either be null or a single string that contains all of a user's set pronouns.

* The ``name-style`` determines how the username is shown on the website and can be one of two things:

  * When ``style`` is ``solid``, there is one ``color``.
  * When ``style`` is ``gradient``, there are two colors that form a linear gradient. The two colors
    are named ``color-from`` and ``color-to``.

  Each color consists of two values. The ``light`` one is to be used on light backgrounds, the
  ``dark`` one is for darker background colors. Choose whatever your app's design needs.

* The user's ``role`` can be one of: ``banned``, ``user``, ``trusted``, ``moderator``, ``admin`` and
  ``programmer``.

* ``signup`` is ``null`` for old user accounts.

* If ``location`` is set (some users live in limbo and have no location), it contains the country
  and possibly region where the user lives. The ``country`` is always set, ``region`` (more specific
  than the country) is not available for all countries and therefore not always set. Each of the two
  contain a ``code`` (which, for the country is the ISO Alpha-2 code, and for the region is
  something custom) and the names (as with users, not all countries/regions have a japanese name).

* Any of the ``twitch``, ``hitbox``, ``youtube``, ``twitter`` and ``speedrunslive`` links can be
  ``null``.
  
* ``assets`` are links to images set by the user on speedrun.com. The links for both ``icon`` and ``image`` can be null.

### GET /users

This will return a list of users. You can filter the result using these query string parameters:

Query Parameter   | Type   | Description
----------------- | ------ | ------------------------------------------------------------------
``lookup``        | string | when given, searches the value (case-insensitive exact-string match) across user names, URLs and social profiles; all other query string filters are disabled when this is given
``name``          | string | only returns users whose name/URL contains the given value; the comparision is case-insensitive
``twitch``        | string | searches for Twitch usernames
``hitbox``        | string | searches for Hitbox usernames
``twitter``       | string | searches for Twitter usernames
``speedrunslive`` | string | searches for SpeedRunsLive usernames

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by               | Description
---------------------- | ------------------------------------------------------------------
``name.int`` (default) | sorts alphanumerically by the international name
``name.jap``           | sorts alphanumerically by the japanese name
``signup``             | sorts by the signup date
``role``               | sorts by the user role

##### Example Requests

* [**GET /api/v1/users**](https://www.speedrun.com/api/v1/users) gets all users
* [**GET /api/v1/users?name=abc**](https://www.speedrun.com/api/v1/users?name=abc) searches for
  all users whose name or URL contains ``abc``
* [**GET /api/v1/users?lookup=pac____**](https://www.speedrun.com/api/v1/users?lookup=pac____) finds
  Pac based on his Twitter account.

##### Example Response

```json
{
  "data": [
    <user>,
    <user>,
    <user>,
    <user>,
    ...
  ]
}
```

### GET /users/{id}

This will retrieve a single user, identified by their ID. Instead of the ID, the username can be
used as well (but this is only recommended for quick lookups, as usernames can change over time), so
``GET /users/Pac`` is possible and will redirect to ``/users/wzx7q875``.

##### Example Requests

* [**GET /api/v1/users/wzx7q875**](https://www.speedrun.com/api/v1/users/wzx7q875) retrieves Pac's
  public user information.

##### Example Response

```json
{
  "data": <user>
}
```

### GET /users/{id}/personal-bests

This will retrieve a list of runs, representing the Personal Bests of the given user. This will not
include obsolete runs. If you want those as well, filter the [global run list](runs.md) by user, for
example ``GET /runs?user=...``.

Instead of the ID, the username can be used as well (for example, ``/users/Pac/personal-bests``).

You can filter the result using these query string parameters:

Query Parameter   | Type   | Description
----------------- | ------ | ------------------------------------------------------------------
``top``           | int    | when given, only PBs with a place equal or better than this value (e.g. ``top=1`` returns all World Records of the given user)
``series``        | string | when given, restricts the result to games and romhacks in that series; can be either a series ID or abbreviation
``game``          | string | when given, restricts the result to that game; can be either a game ID or abbreviation

The returned structure is a flat list of ranked runs, similar to the [leaderboards](leaderboards.md):

```json
{
  "data": [{
    "place": 7,
    "run": <run>
  }, {
    "place": 2,
    "run": <run>
  }, ...]
}
```

All embeds that are available for runs are available here as well (``game``, ``category`` etc.). Note
that they are not embedded in the run, but placed beside it. ``?embed=game,category`` results therefore
in a structure like this:

```json
{
  "data": [{
    "place": 7,
    "run": <run>,
    "game": <game>,
    "category": <category>
  }, {
    "place": 2,
    "run": <run>,
    "game": <game>,
    "category": <category>
  }, ...]
}
```

##### Example Requests

* [**GET /api/v1/users/wzx7q875/personal-bests**](https://www.speedrun.com/api/v1/users/wzx7q875/personal-bests)
  retrieves Pac's Personal Bests.
* [**GET /api/v1/users/wzx7q875/personal-bests?embed=game,category**](https://www.speedrun.com/api/v1/users/wzx7q875/personal-bests?embed=game,category)
  makes Pac's PBs much more useful to actually work with by embedding the game and category for each
  run.

##### Example Response

```json
{
  "data": [{
    "place": 38,
    "run": <run>,
    "game": <game>,
    "category": <category>
  }, {
    "place": 1,
    "run": <run>,
    "game": <game>,
    "category": <category>
  }, ...]
}
```
