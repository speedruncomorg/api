# Users

* [Structure](#structure)
* [GET /users](#get-users)
* [GET /users/{id}](#get-usersid)

Users are the individuals who have registered an account on speedrun.com. Users submit (their)
[runs](runs.md) and moderate [games](games.md), besides other things that are not covered by this
API (like writing posts in the forums).

The API only returns public user information, so things like the e-mail address, site settings etc.
are not accessible.

### Structure

Represented as JSON, a single user looks like this:

```json
{
  "id": 1274,
  "weblink": "http://www.speedrun.com/user/chewdiggy",
  "names": {
    "international": "chewdiggy",
    "japanese": null
  },
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
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/users/1274"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?user=1274"
  }, {
    "rel": "games",
    "uri": "http://www.speedrun.com/api/v1/games?moderator=1274"
  }]
}
```

There are quite a few things that need a couple words of explanation.

* Most users don't have a ``japanese`` name.

* The ``name-style`` determines how the username is shown on the website and can be one of two things:

  * When ``style`` is ``solid``, there is one ``color``.
  * When ``style`` is ``gradient``, there are two colors that form alinear gradient. The two colors
    are named ``color-from`` and ``color-to``.

  Each color consists of two values. The ``light`` one is to be used on light backgrounds, the
  ``dark`` one is for darker background colors. Choose whatever your app's design needs.

* The user's ``role`` can be one of: ``banned``, ``user``, ``trusted``, ``moderator``, ``admin`` and
  ``programmer``.

* ``signup`` is ``null`` for old user accounts.

* ``location`` contains the country and possibly region where the user lives. The ``country`` is
  always set, ``region`` (more specific than the country) is not available for all countries and
  therefore not always set. Each of the two contain a ``code`` (which, for the country is the
  ISO Alpha-2 code, and for the region is something custom) and the names (as with users, not all
  countries/regions have a japanese name).

### GET /users

This will return a list of users. You can filter the result using these query string parameters:

Query Parameter  | Type   | Description
---------------- | ------ | ------------------------------------------------------------------
``name``         | string | only returns users whose name/URL contains the given value; the
                 |        | comparision is case-insensitive

##### Example Requests

* [**GET /api/v1/users**](http://www.speedrun.com/api/v1/users) gets all users
* [**GET /api/v1/users?name=abc**](http://www.speedrun.com/api/v1/users?name=abc) searches for
  all users whose name or URL contains ``abc``

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

This will retrieve a single user, identified by their ID.

##### Example Requests

* [**GET /api/v1/users/1**](http://www.speedrun.com/api/v1/users/1) retrieves Pac's public user
  information.

##### Example Response

```json
{
  "data": <user>
}
```
