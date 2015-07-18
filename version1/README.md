# Version 1

This version is available at **http://www.speedrun.com/api/v1**.

:warning: **Version 1 is not yet stable and can change at any time.** :warning:

## Basics

* All dates and times are [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) encoded. Durations
  (like run times) use both ISO 8601 as well as the number of seconds and milliseconds (use whatever
  is convenient for you).
* In most responses, the JSON structure contains a ``data`` key as its root element. This ``data``
  element is omitted from the sample JSONs on this page.
* By default, all collections are limited to 20 elements. Check the [pagination](pagination.md)
  documentation for more information.

## Resources

This version offers access to the following resources:

* [Categories](categories.md)
* [Games](games.md)
* [Guests](guests.md)
* [Leaderboards](leaderboards.md)
* [Levels](levels.md)
* [Platforms](platforms.md)
* [Profile](profile.md)
* [Regions](regions.md)
* [Runs](runs.md)
* [Series](series.md)
* [Users](users.md)
* [Variables](variables.md)

## Examples

### Get a specific, complete Leaderboard

This will retrieve the **96 Exit** leaderboard for Super Mario World:

[**GET /api/v1/leaderboards/smw/category/96_Exit**](http://www.speedrun.com/api/v1/leaderboards/smw/category/96_Exit)

Note that you will be redirected, because the API wants you to not use the ephemeral game abbreviations
(``smw``), but rather their fixed IDs. It is recommended to use IDs whenever possible.

### Get the World Record in a given Game

This will retrieve the **70 Stars** World Record for Super Mario 64:

* [**GET /api/v1/leaderboards/sm64/category/70_Star?top=1**](http://www.speedrun.com/api/v1/leaderboards/sm64/category/70_Star?top=1) or
* [**GET /api/v1/games/sm64/records?top=1**](http://www.speedrun.com/api/v1/games/sm64/records?top=1) to fetch the records for all
  categories/levels (70 Star, 120 Star, ...) at once.

Note that you will be redirected, because the API wants you to not use the ephemeral game abbreviations
(``smw``), but rather their fixed IDs. It is recommended to use IDs whenever possible.

### Get Somebody's Personal Bests

This will retrieve all PBs by S.:

[**GET /api/v1/users/S./personal-bests**](http://www.speedrun.com/api/v1/users/S./personal-bests)

Note that you will be redirected, because the API wants you to not use the ephemeral username
(``S.``), but rather their fixed IDs. It is recommended to use IDs whenever possible.

### Find all Runs done on the iQue

Note that this includes obsolete runs and that you get no ranking information with this resource. If
you need ranks, request a user's PBs or leaderboards.

[**GET /api/v1/runs?region=mol4z19n**](http://www.speedrun.com/api/v1/runs?region=mol4z19n)

### Find all Games available on the Wii U

[**GET /api/v1/games?platform=8zjw7vo6**](http://www.speedrun.com/api/v1/games?platform=8zjw7vo6)
