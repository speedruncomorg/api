# Notifications

* [Structure](#structure)
* [GET /notifications](#get-notifications)

Notifications are system-generated messages sent to [users](users.md) when certain events concerning
them happen on the site, like somebody liking a post or a [run](runs.md) being verified.

### Structure

Represented as JSON, a single notification looks like this:

```json
{
  "id": "p2p5rrle",
  "created": "2015-01-25T11:55:15Z",
  "status": "read",
  "text": "Lighnat0r likes your post.",
  "item": {
    "rel": "post",
    "uri": "http://www.speedrun.com/post/ptv8h"
  },
  "links": [{
    "rel": "run",
    "uri": "http://www.speedrun.com/api/v1/runs/1wzpqgyq"
  }, {
    "rel": "game",
    "uri": "http://www.speedrun.com/api/v1/games/nj1ne1p4"
  }]
}
```

There are quite a few things that need a couple words of explanation.

* The ``status`` can be either ``read`` or ``unread``.
* There are four different kinds of items that are linked to a notification: ``post`` (someone liked
  the forum post), ``run``, ``game`` (when a game request was approved/denied) and ``guide``
  (when a guide was updated).
* If the notification concerns a run, ``links`` *can* contain a link to the run resource. In some
  cases, the game/run has already been removed again, so there's not always a link possible.
* If the notification concerns a game, ``links`` *can* contain a link to the game resource. In some
  cases, the game has already been removed again, so there's not always a link possible.
* The example above shows both links, even though there can be only one of them at any time.
* When the referenced item is gone/lost, the ``item``'s ``uri`` points to the homepage.

### GET /notifications

This will retrieve the notifications for the currently [authenticated](../authentication.md) user.
By default, the list is sorted by date descending. You can control the sorting by using the query
string parameters ``orderby`` and ``direction``. The direction can be either ``asc`` or ``desc``,
the possible values for ``orderby`` are listed below.

order by               | Description
---------------------- | ------------------------------------------------------------------
``created`` (default)  | sorts by the creation date of the notification

##### Example Requests

* **GET /api/v1/notifications**

##### Example Response

```json
{
  "data": [
    <notification>,
    <notification>,
    <notification>,
    ...
  ]
}
```
