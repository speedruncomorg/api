# Pagination

Collections like runs, games or users do not return all elements at once, but are limited to a
certain number per request. By default, you get **20 elements** per request, but you can inscrease
this limit **up to 200**.

Collections will not only have a ``data`` element, but also a ``pagination`` element in their JSON
representation. This pagination element contains the currently used limit, offset and links to the
previous and next pages.

```json
{
  "data": [<user>, <user>, <user>],
  "pagination": {
    "offset": 20,
    "max": 30,
    "size": 30,
    "links": [{
      "rel": "next",
      "uri": "http://speedrun.gg/api/v1/users?max=30&offset=50"
    }, {
      "rel": "prev",
      "uri": "http://speedrun.gg/api/v1/users?max=30"
    }]
  }
}
```

The ``size`` value contains the number of elements in the current response, so it's always between
0 and ``max``.

### Navigating through collections

The number of elements per request can be set using the ``max`` query string parameter (e.g.
``GET /users?max=50``). Values from 1 to 200 are acceptable.

To advance to the next set of elements, use the ``offset`` parameter (e.g. ``GET /users?offset=40``).
The offset starts at 0 and can be set independently of the ``max`` value.

* ``GET /users`` - first 20 users
* ``GET /users?max=50`` - first 50 users
* ``GET /users?offset=20`` - users 21 to 40
* ``GET /users?offset=20&max=5`` - users 21 to 25

Instead of manually constructing these parameters, it is recommended that API clients use the
available links that the API offers. This makes API clients more reobust in case we change the
pagination in a future API version.

The API does not provide the total number of elements for each collection, so API clients must fetch
all pages until no more results are available. Check for ``pagination.size`` being less than the
``max`` amount of elements you want to detect the last page.
