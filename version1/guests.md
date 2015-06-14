# Guests

* [Structure](#structure)
* [GET /v1/guests/{name}](#get-v1guestsname)

Sometimes, speedrun.com has runs done by players that have no account on the site yet. These runners
are called "guests" in the API. Except for a name, there is nothing we know about them.

### Structure

Represented as JSON, a single guest looks like this:

```json
{
  "name": "Alex",
  "links": [{
    "rel": "self",
    "uri": "http://www.speedrun.com/api/v1/guests/Alex"
  }, {
    "rel": "runs",
    "uri": "http://www.speedrun.com/api/v1/runs?guest=Alex"
  }]
}
```

### GET /v1/guests/{name}

This will retrieve a guest, identified by their name. The name is case-insensitive.

##### Example Requests

* [**GET /api/v1/guests/Alex**](http://www.speedrun.com/api/v1/guests/Alex) retrieves someone named
  "Alex".

##### Example Response

```json
{
  "data": <guest>
}
```
