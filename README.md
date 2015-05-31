# speedrun.com REST API

We offer a simple, JSON-ased REST API to access most of our data about games, runs and players. The
API is meant to be a good compromise between pure REST and real-world.

If you want to use the API, you should be familiar with how HTTP and JSON work. Documenting those
basics is out of scope of this documentation.

## Basics

Most of the API is **read-only** and **anonymous**, i.e. you don't need any credentials or
permissions to access the API. We add support for write operations on a case-by-case basis,
depending on our needs. In those cases, API clients need to authenticate by using a user's API key
(each user has one and if you want to do stuff on behalf of other users, they need to give their
API key to your app).

The API is versioned, the version is reflected in the URL you use. The API root,
http://www.speedrun.com/api will redirect you to the current version (v1 as of writing this). We
promise to do our best to never break a once published version of our API (sometimes we extend the
current API with new information, but we will not change existing fields).

## Versions

* [Version 1](https://github.com/speedruncom/api/blob/master/version1/README.md)
