# Faeria event log proposal (WIP)

To ease the task of measuring the performance of a Faeria player and their decks, an automated tracking mechanism is desired. Creating an API to provide statistics and match replays would be the ideal solution, but it can be too much too ask. Instead,  this document proposes an event log to be parsed by 3rd party tools created by the community.

## Log format

A log is a plain text file where each line represents one event serialized as JSON.

The basic structure of an event is:

* `uuid`: A UUID.
* `at`: An ISO 8601 date that represents when the event ocurred.
* `type`: A string representing the type of the event.
* `attributes`: An additional JSON object with attributes that are dependent to the specific event type.

For example (formatted for legibility):

```json
{
  "uuid": "d3d66c60-b899-4e81-8777-4b074567c9ed",
  "at": "2016-11-02T20:45:48Z",
  "type": "EMPTY",
  "attributes": {
  }
}
```

## Events

The logging for different events can be incrementally added, for example just having events for a match starting and ending would be enough to have the winrate and average match duration for a player. 

### Match started

The `type` is `MATCH_STARTED` and the `attributes` are:

* `format`: A string representing the match format (`RANKED`, `UNRANKED`, `PHANTOM_PANDORA`, `PANDORA`).
* `player`: A string with the handle of the player.
* `enemy`: A string with the handle of the enemy player.
* `first`: A boolean that indicates whether the player plays first or not.

For example (formatted for legibility):

```json
{
  "uuid": "c25aab3f-9a5f-4294-acfe-c12b49d668b7",
  "at": "2016-11-02T20:45:48Z",
  "type": "MATCH_STARTED",
  "attributes": {
    "format": "RANKED",
    "player": "gutsyfrog",
    "enemy": "hiroshi",
    "first": true
  }
}
```

### Match ended

The `type` is `MATCH_ENDED` and the `attributes` are:

* `format`: A string representing the match format (`RANKED`, `UNRANKED`, `PHANTOM_PANDORA`, `PANDORA`).
* `player`: A string with the handle of the player.
* `enemy`: A string with the handle of the enemy player.
* `won`: A boolean that indicates whether the player won or not.

For example (formatted for legibility):

```json
{
  "uuid": "d600751b-2603-4b7a-b133-bfada49b8943",
  "at": "2016-11-02T20:56:27Z",
  "type": "MATCH_ENDED",
  "attributes": {
    "format": "RANKED",
    "player": "gutsyfrog",
    "enemy": "hiroshi",
    "won": true
  }
}
```

## TODO

* Add events that enable tracking which cards were played.
* Add events that enable replays.
