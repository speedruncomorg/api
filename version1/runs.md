# Runs

* [Structure](#structure)
* [Embeds](#embeds)
* [GET /runs](#get-runs)
* [GET /runs/{id}](#get-runsid)

Runs are the meat of our business at speedrun.com. A run is a finished attempt to play a
[game](games.md), adhering to that game's ruleset. Invalid attempts (use of cheats) or obsolete
runs (the ones superseded by a better time by the same player(s) in the same ruleset) still count as
runs and are available via API.

If you need the **leaderboards** (non-obsolete, valid runs sorted by time), this is not the droid
you're looking for. Until we had time to properly migrate the leaderboards to this API, use the
legacy API available at http://www.speedrun.com/api_records.php.

### Structure

Represented as JSON, a single run looks like this:

```json
{
  "id": 3128,
  "weblink": "http://www.speedrun.com/run/3128",
  "game": 253,
  "level": null,
  "category": 527,
  "video": "http://youtube.com/videoidhere",
  "comment": "Lost 35 seconds on bridge swap, plus a bunch of time elsewhere",
  "status": {
    "status": "verified",
    "examiner": 62
  },
  "players": [
    1316,
    "Betsruner"
  ],
  "date": "2014-06-01",
  "submitted": null,
  "times": {
    "primary": "PT31M22S",
    "primary_t": 1882,
    "realtime": "PT31M22S",
    "realtime_t": 1882,
    "realtime_noloads": "PT41M26S",
    "realtime_noloads_t": 2486,
    "ingame": null,
    "ingame_t": 0
  },
  "system": {
    "platform": 31,
    "emulated": false,
    "region": null
  },
  "values": {
    "1497": 4099
  },
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/runs/3128"
  }, {
    "rel": "game",
    "uri": "http://www.speedrun.com/api/v1/games/253"
  }, {
    "rel": "category",
    "uri": "http://www.speedrun.com/api/v1/categories/527"
  }, {
    "rel": "level",
    "uri": "http://www.speedrun.com/api/v1/levels/9610"
  }, {
    "rel": "platform",
    "uri": "http://www.speedrun.com/api/v1/platforms/31"
  }, {
    "rel": "player",
    "uri": "http://www.speedrun.com/api/v1/users/1316"
  }, {
    "rel": "player",
    "uri": "http://www.speedrun.com/api/v1/guests/Betsruner"
  }, {
    "rel": "examiner",
    "uri": "http://www.speedrun.com/api/v1/users/62"
  }]
}
```

There are quite a few things that need a couple words of explanation.

* ``status`` is a structure containing the status and possibly the ID of the [user](users.md) that
  made the status decision. The status can be ``new`` (not yet reviewed), ``verified`` or
  ``rejected``. If the status is ``new``, the field ``examiner`` is not present. ``rejected`` runs
  additionally have a ``reason`` string field within their status, containing the reason message by
  the examiner.
* ``players`` is the list of players that participated in the run. Each player can either be a
  registered user (in that case, the player is an int value, containing the user ID) or a guest of
  whom we only have a name, but no user account (in that case, it's a string).

  Use the ``links`` to find the user profiles or guest profiles of the runners. Note that guests
  are **not** available via the [users resource](users.md), but have their own [guests resource](guests.md).

* ``date`` is when the run happened. Not all runs have a known date, so unfortunately this sometimes
  is ``null``. ``submitted`` is the date and time when the run was added on speedrun.com and can
  also be ``null`` for old runs.

* ``times`` is a structure that contains a lot of information and a lot of redundant stuff.

  * ``primary`` is the time that is relevant for the leaderboard. Different games have different
    rules as to what eventually counts, and this is the time represented by ``primary``.
  * ``realtime`` is the real-world time of the run.
  * ``realtime_noloads`` is the real-world time of the run, *excluding* the loading times. Not all
    games have a distinction between realtime and realtime w/o loads.
  * ``ingame`` is the time as measured by the game itself.

  Each of those four times is represented twice in the ``times`` structure. Once as a ISO 8601 duration
  and once as the number of seconds plus milliseconds (as a float). Use whatever is the easiest in
  your environment and try to avoid implementing custom date/time parsing.

  The primary time is always set, each of the three others can be empty depending on the game.

* ``system.region`` can be null, especially for PC games.

* ``values`` is a mapping from [variables](variables.md) to values (the key is the variable ID, the
  value is the valueID). The map represents the custom values for a game (like the speed in Mario Kart,
  which can be 50/100/150cc).

* Not all runs have a ``video``. For those, the field is ``null``. Sames goes for the ``comment``.
  Also, the video can be pretty much anything that's acceptable to the examiner of a run, so don't
  rely on this being a YouTube link.

### Embeds

You can [embed](embedding.md) the following resources into a run:

* ``game`` will embed the full game resource.

### GET /runs

This will return a list of all runs. You can filter the result by a few things:

Query Parameter  | Type   | Description
---------------- | ------ | ------------------------------------------------------------------
``user``         | int    | user ID; when given, only returns runs played by that user
``guest``        | string | when given, only returns runs done by that guest
``examiner``     | int    | user ID; when given, only returns runs examined by that user
``game``         | int    | game ID; when given, restricts to that game
``level``        | int    | level ID; when given, restricts to that level
``category``     | int    | category ID; when given, restricts to that category
``platform``     | int    | platform ID; when given, restricts to that platform
``region``       | int    | region ID; when given, restricts to that region
``emulated``     | bool   | when ``1``, ``yes`` or ``true``, only games run on emulator will be returned
``status``       | string | filters by run status; ``new``, ``verified`` and ``rejected`` are possible values for this parameter

Note that giving invalid values for any ID parameter will result in an HTTP 404 error instead of an
empty list. This is on purpose, because asking to filter by non-existing elements should be
something an API client should notice.

##### Example Requests

* [**GET /api/v1/runs**](http://www.speedrun.com/api/v1/runs) gets all runs
* [**GET /api/v1/runs?guest=Alex**](http://www.speedrun.com/api/v1/runs?guest=Alex) searches for
  all runs done by someone named "Alex" that has no speedrun.com account.
* [**GET /api/v1/runs?emulated=yes&examiner=1**](http://www.speedrun.com/api/v1/runs?emulated=yes&examiner=1)
  searches for all runs done via emulator that have been examined by Pac.

##### Example Response

```json
{
  "data": [
    <run>,
    <run>,
    <run>,
    <run>,
    ...
  ]
}
```

### GET /runs/{id}

This will retrieve a single run, identified by its ID.

##### Example Requests

* [**GET /api/v1/runs/2**](http://www.speedrun.com/api/v1/runs/2) retrieves m00nchile's GTA VC run.

##### Example Response

```json
{
  "data": <run>
}
```
