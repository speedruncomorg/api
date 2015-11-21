# Runs

* [Structure](#structure)
* [Embeds](#embeds)
* [GET /runs](#get-runs)
* [GET /runs/{id}](#get-runsid)
* [POST /runs](#post-runs)

Runs are the meat of our business at speedrun.com. A run is a finished attempt to play a
[game](games.md), adhering to that game's ruleset. Invalid attempts (use of cheats etc) or obsolete
runs (the ones superseded by a better time by the same player(s) in the same ruleset) still count as
runs and are available via API.

### Structure

Represented as JSON, a single run looks like this:

```json
{
  "id": "90y6pm7e",
  "weblink": "http://www.speedrun.com/run/90y6pm7e",
  "game": "29d30dlp",
  "level": null,
  "category": "nxd1rk8q",
  "videos": {
    "text": "Part 1: http://youtube.com/videoidhere -- HYPE!!",
    "links": [{
      "uri": "http://youtube.com/videoidhere"
    }, {
      "uri": "http://youtube.com/anothervideo"
    }]
  },
  "comment": "Lost 35 seconds on bridge swap, plus a bunch of time elsewhere. Part 2 is http://youtube.com/anothervideo",
  "status": {
    "status": "verified",
    "examiner": "61xymxr9",
    "verify-date": "2015-05-23T16:12:32Z"
  },
  "players": [{
    "rel": "user",
    "id": "61xymxr9",
    "uri": "http://www.speedrun.com/api/v1/users/61xymxr9"
  }, {
    "rel": "guest",
    "name": "Betsruner",
    "uri": "http://www.speedrun.com/api/v1/guests/Betsruner"
  }],
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
    "platform": "rdjq4vwe",
    "emulated": false,
    "region": null
  },
  "splits": {
    "rel": "splits.io",
    "uri": "https://splits.io/api/v3/runs/55b"
  },
  "values": {
    "wvn9wp51": "ezj43ozx"
  },
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/runs/90y6pm7e"
  }, {
    "rel": "game",
    "uri": "http://www.speedrun.com/api/v1/games/29d30dlp"
  }, {
    "rel": "category",
    "uri": "http://www.speedrun.com/api/v1/categories/nxd1rk8q"
  }, {
    "rel": "level",
    "uri": "http://www.speedrun.com/api/v1/levels/hhzd4bym"
  }, {
    "rel": "platform",
    "uri": "http://www.speedrun.com/api/v1/platforms/rdjq4vwe"
  }, {
    "rel": "examiner",
    "uri": "http://www.speedrun.com/api/v1/users/61xymxr9"
  }]
}
```

There are quite a few things that need a couple words of explanation.

* ``status`` is a structure containing the status and possibly the ID of the [user](users.md) that
  made the status decision. The status can be ``new`` (not yet reviewed), ``verified`` or
  ``rejected``. If the status is ``new``, the field ``examiner`` is not present. ``rejected`` runs
  additionally have a ``reason`` string field within their status, containing the reason message by
  the examiner. ``verified`` runs have a ``verify-date`` field that contains the date when the run
  was verified. This date can be ``null`` for runs that have been verified in the Old Days.

* ``players`` is the list of players that participated in the run. Each player can either be a
  registered user (in that case, the ``rel`` value is ``user`` and there is an ``id`` present) or a
  guest of whom we only have a name, but no user account (in that case, ``rel`` is ``guest`` and
  the ``name`` is present). In both cases, a ``uri`` is present that links to the player resource.

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

* ``videos`` contains information about all video links that have been provided for the run. This
  includes the primary video (most runs only have one video, some have none at all) and all videos
  mentioned in the ``comments``.

  If the primary video link contains nothing resembling a link or contains more text than just a
  link, the full text is available as ``videos.text``. Otherwise, ``videos.text`` is not set.

  If there are no videos and no video fallback text at all, ``videos`` is ``null``.

  Links to the following sites are recognized as video links: twitch.tv, youtube.com, youtu.be,
  hitbox.tv, vimeo.com and nicovideo.jp.

* ``splits`` contains information about the detailed timing of the run. This field can either be
  ``null`` or a link to the splits. Currently, we support [splits.io](https://www.splits.io), but
  in the future, the link might point to other sites as well, so either blindly follow the link or
  pay attention to the ``rel`` field.

### Embeds

You can [embed](embedding.md) the following resources into a run:

* ``game`` will embed the full game resource.
* ``category`` will embed the category in which the run was done.
* ``level`` will embed the level in which the run was done. This can be empty if it's a full-game
  run.
* ``players`` will replace the ``players`` field with the full [user](users.md)/[guest](guests.md)
  resources of the participating players. Each of those players will have the ``rel`` field just as
  if there was no embedding at all, so you can easily distinguish between users and guests (without
  resorting to logic like "if there's an ID, it must be a user").
* ``region`` will embed the full region resource. This can be empty if no region was set for the run.
* ``platform`` will embed the full platform resource. This can be empty if no platform was set for
  the run.

### GET /runs

This will return a list of all runs. You can filter the result by a few things:

Query Parameter  | Type   | Description
---------------- | ------ | ------------------------------------------------------------------
``user``         | string | user ID; when given, only returns runs played by that user
``guest``        | string | when given, only returns runs done by that guest
``examiner``     | string | user ID; when given, only returns runs examined by that user
``game``         | string | game ID; when given, restricts to that game
``level``        | string | level ID; when given, restricts to that level
``category``     | string | category ID; when given, restricts to that category
``platform``     | string | platform ID; when given, restricts to that platform
``region``       | string | region ID; when given, restricts to that region
``emulated``     | bool   | when ``1``, ``yes`` or ``true``, only games run on emulator will be returned
``status``       | string | filters by run status; ``new``, ``verified`` and ``rejected`` are possible values for this parameter

Note that giving invalid values for any ID parameter will result in an HTTP 404 error instead of an
empty list. This is on purpose, because asking to filter by non-existing elements should be
something an API client should notice.

You can control the sorting by using the query string parameters ``orderby`` and ``direction``. The
direction can be either ``asc`` or ``desc``, the possible values for ``orderby`` are listed below.

order by            | Description
------------------- | ------------------------------------------------------------------
``game`` (default)  | sorts by the game the run was done in
``category``        | sorts by the category the run was done in
``level``           | sorts by the level the run was done in
``platform``        | sorts by the console used for the run
``region``          | sorts by the console region the run was done in
``emulated``        | sorts by whether or not a run is done via emulator
``date``            | sorts by the date the run happened on
``submitted``       | sorts by the date when the run was submitted to speedrun.com
``status``          | sorts by verification status
``verify-date``     | sorts by the date the run was verified on

##### Example Requests

* [**GET /api/v1/runs**](http://www.speedrun.com/api/v1/runs) gets all runs
* [**GET /api/v1/runs?status=verified&orderby=verify-date&direction=desc**](http://www.speedrun.com/api/v1/runs?status=verified&orderby=verify-date&direction=desc)
  gets all newly verified runs
* [**GET /api/v1/runs?status=new&orderby=submitted&direction=desc**](http://www.speedrun.com/api/v1/runs?status=new&orderby=submitted&direction=desc)
  gets the newest runs (based on the submit date, not the run date)
* [**GET /api/v1/runs?guest=Alex**](http://www.speedrun.com/api/v1/runs?guest=Alex) searches for
  all runs done by someone named "Alex" that has no speedrun.com account.
* [**GET /api/v1/runs?emulated=yes&examiner=wzx7q875**](http://www.speedrun.com/api/v1/runs?emulated=yes&examiner=wzx7q875)
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

* [**GET /api/v1/runs/90y6pm7e**](http://www.speedrun.com/api/v1/runs/90y6pm7e) retrieves m00nchile's
  GTA VC run.

##### Example Response

```json
{
  "data": <run>
}
```

### POST /runs

*This method requires [authentication](../authentication.md).*

By POSTing to the runs collection, you can submit a new run to speedrun.com. The same rules as for
submitting via the website apply, meaning that for example moderators can auto-verify their runs
immediately.

**Note:** Not all features are yet implemented. Runs for example can only be done by one player, the
user that is submitting the run.

The request body must be a JSON object describing the run. This should look like this:

```json
{
  "run": {
    "category": "<category ID>",
    "level": "<level ID>",
    "date": "YYYY-MM-DD",
    "region": "<region ID>",
    "platform": "<platform ID>",
    "verified": false,
    "times": {
      "realtime": 1234.56,
      "realtime_noloads": 1200.10,
      "ingame": 1150
    },
    "emulated": false,
    "video": "https://youtube.com/watch?v=mumblefoo",
    "comment": "Most awesome run ever!",
    "splitsio": "6vr",
    "variables": {
      "<variable ID>": {
        "type": "user-defined",
        "value": "my value"
      },
      "<variable ID>": {
        "type": "pre-defined",
        "value": "<value ID>"
      }
    }
  }
}
```

A few things worth noting:

* ``category`` is mandatory, ``level`` only has to be given for individual-level runs.
* ``date`` is optional and defaults to the current date.
* ``region`` is optional, just like ``platform``. Some games require proper information on these
  fields, so always try to provide them.
* ``verified`` can only be set if the submitting user is a moderator of the game.
* ``times`` must contain at least one time (of the times that the game allows, obviously). This
  value is given as the number of seconds as a int/float, optionally (when the games allows it,
  otherwise the value will be rounded automatically) including milliseconds.
* ``video`` must be a valid URL using HTTP or HTTPS as its protocol. Note that some games require
  a video to be included.
* ``comment`` is optional, but highly encouraged. You can put additional video links in here, if you
  have multiple Twitch VODs for example.
* ``splitsio`` is optional. If given, it should be the splits.io ID or a full URL to the splits.
* ``variables`` contains the variable values for the new run. Some games have mandatory variables.
  Depending on the variable type, you can give the ID of a pre-defined (i.e. previously submitted
  by someone) value or submit a new one.

See the [JSON schema](json-schema/run-submit.json) for more details.

##### Example Response

When successful, HTTP status code 201 is returned. The response contains a ``Location`` header
pointing to the new run, as well as the run as the response body as well.

```json
{
  "data": <run>
}
```

If something goes wrong, the response will contain a list of problems:

```json
{
  "status": 400,
  "message": "The submitted run does not validate against the schema. See the `errors` attached.",
  "errors": [
    "[category] is missing and it is required",
    "[platform] is missing and it is required",
    "[times] is missing and it is required"
  ],
  "links": [
    {
      "rel": "support",
      "uri": "irc://speedrunslive.com#speedrun.com"
    },
    {
      "rel": "report-issues",
      "uri": "https://github.com/speedruncom/api/issues"
    }
  ]
}
```
