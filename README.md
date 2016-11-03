# Faeria event log proposal (WIP)

To ease the task of measuring the performance of a Faeria player and their decks, an automated tracking mechanism is desired.

An API providing statistics and match replays is the ideal solution but, given the amount of effort necessary, it may not be feature of high priority for the Faeria development team. Instead, this document proposes a client side event log that can be parsed by 3rd party tools created by the community.

The events can be incrementally added in new patches of the client, providing new levels of detail. For example:

* Iteration 1: Add an event for a match ending, indicating the winner. This makes possible to track the winrate of a player.
* Iteration 2: Add an event for a match starting. By substracting the timestamps of the match ending and starting events, it's possible to track match duration.
* Iteration 3: Add an event for card draws. This makes possible to deduce the archetypes of the decks being played.
* Future iterations: Add events for cards played, lands created, etc. Until the information is enough to re-play full matches.

## Log format

A log is a plain text file where each line represents one event serialized as JSON.

The basic structure of an event is:

* `uuid`: A UUID.
* `at`: An ISO 8601 timestamp with millisecond precision that represents when the event ocurred.
* `type`: A string representing the type of the event.
* `attributes`: An additional JSON object with attributes that are dependent to the specific event type.

For example (formatted for legibility):

```json
{
  "uuid": "d3d66c60-b899-4e81-8777-4b074567c9ed",
  "at": "2016-11-02T20:45:48.123Z",
  "type": "EMPTY",
  "attributes": {
  }
}
```

## Events

### Match started

The `type` is `MATCH_STARTED` and the `attributes` are:

* `format`: A string representing the match format (`RANKED`, `UNRANKED`, `PHANTOM_PANDORA`, `PANDORA`).
* `player`: A string with the handle of the player.
* `opponent`: A string with the handle of the opponent.
* `first`: A boolean that indicates whether the player plays first or not.

For example (formatted for legibility):

```json
{
  "uuid": "c25aab3f-9a5f-4294-acfe-c12b49d668b7",
  "at": "2016-11-02T20:45:48.654Z",
  "type": "MATCH_STARTED",
  "attributes": {
    "format": "RANKED",
    "player": "gutsyfrog",
    "opponent": "hiroshi",
    "first": true
  }
}
```

### Match ended

The `type` is `MATCH_ENDED` and the `attributes` are:

* `format`: A string representing the match format (`RANKED`, `UNRANKED`, `PHANTOM_PANDORA`, `PANDORA`).
* `player`: A string with the handle of the player.
* `opponent`: A string with the handle of the opponent.
* `won`: A boolean that indicates whether the player won or not.

For example (formatted for legibility):

```json
{
  "uuid": "d600751b-2603-4b7a-b133-bfada49b8943",
  "at": "2016-11-02T20:56:27.058Z",
  "type": "MATCH_ENDED",
  "attributes": {
    "format": "RANKED",
    "player": "gutsyfrog",
    "opponent": "hiroshi",
    "won": true
  }
}
```

## TODO

* Add events that enable tracking which cards were played.
* Add events that enable replays.
