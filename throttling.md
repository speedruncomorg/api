# Throttling

To prevent excessive crawling of the data we make available and to ensure a good overall performance,
requests to the API are subject to rate limits.

Each IP is allowed to perform **100 requests per minute**. We observe the usage and user requirements
and may adjust this if needed. At the moment, IPs can make new requests as soon as the number of
requests within the last minute sinks below 100 again (so you do not get blocked for a certain amount
of time).

To reduce the amount of requests you make, consider:

* [embedding](version1/embedding.md) resources or
* increase the amount of [elements per page](version1/pagination.md) in a collection to the maximum
  allowed value (200).

When you reached your request limit, the API will respond with the HTTP status code ``420``.

If you need to make more requests than allowed, [contact us](http://www.speedrun.com/about) and we
can find a solution.
