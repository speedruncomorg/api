# Runs

* [Structure](#structure)
* [GET /v1/games](#get-v1games)
* [GET /v1/games/{id}](#get-v1gamesid)
* [GET /v1/games/{id}/categories](#get-v1gamesidcategories)
* [GET /v1/games/{id}/levels](#get-v1gamesidlevels)

Runs are the meat of our business at speedrun.com. A run is a finished attempt to play a game,
adhering to that game's ruleset. Invalid attempts (use of cheats) or obsolete runs (the ones
superseded by a better time by the same player(s) in the same ruleset) still count as runs and are
available via API.

If you need the **leaderboards** (non-obsolete, valid runs sorted by time), this is not the droid
you're looking for. Until we had time to properly migrate the leaderboards to this API, use the
legacy API available at http://www.speedrun.com/api_records.php.

### Structure

Represented as JSON, a single run looks like this:

```json
{
  "id": 3128,
  "weblink": "/run/3128",
  "game": 253,
  "level": null,
  "category": 527,
  "video": null,
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
  "values": [],
  "links": [{
    "rel": "self",
    "uri": "/v1/runs/3128"
  }, {
    "rel": "game",
    "uri": "/v1/games/253"
  }, {
    "rel": "category",
    "uri": "/v1/categories/527"
  }, {
    "rel": "platform",
    "uri": "/v1/platforms/31"
  }, {
    "rel": "player",
    "uri": "/v1/users/1316"
  }, {
    "rel": "player",
    "uri": "/v1/guests/Betsruner"
  }, {
    "rel": "examiner",
    "uri": "/v1/users/62"
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

* ``system/region`` can be null, especially for PC games.

* ``values`` is a map containing the custom values for a game (like the speed in Mario Kart, which
  can be 50/100/150cc).
