# Schemas

## ResponseHeader

```json
{
  "title": "",
  "generated_time": "",
  "url": ""
}
```

| Name           | Type     | Description                                                  |
| -------------- | -------- | ------------------------------------------------------------ |
| title          | string   | The title of this response.                                  |
| generated_time | datetime | The date and time this response was generated. In [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format. |
| url            | string   | The relative URL representing this response.                 |
| error          | string   | (Optional) The details of any error that occurred while servicing the request. |

## MeetingsResponse

```json
{
  "header": {...},
  "params": {...},
  "data": {...}
}
```

| Name   | Type                                            | Description                          |
| ------ | ----------------------------------------------- | ------------------------------------ |
| header | [ResponseHeader](#responseheader)               | Meta data describing the response.   |
| params | [MeetingsRequestParams](#meetingsrequestparams) | The parameters sent in this request. |
| data   | [MeetingsData](#meetingsdata)                   | The meeting data.                    |

## MeetingsRequestParams

```json
{
  "enc": "json",
  "id": "",
  "date_from": "",
  "date_to": "",
  "type": " ",
  "country": " ",
  "limit": 100,
  "offset": 0
}
```

See [Meeting Request Parameters](#meeting-request-parameters).

| Name      | Type     | Description                                                  |
| --------- | -------- | ------------------------------------------------------------ |
| enc       | string   | The encoding format for the response.                        |
| id        | string   | UUIDv4 of the meeting requested, if applicable.              |
| date_from | datetime | The date and time to request meetings after. In [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format. |
| date_to   | datetime | The date and time to request meetings until. In [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format. |
| category  | string   | Limit the response to the requested category, if applicable. |
| country   | string   | Limit the response to the requested country, if applicable.  |
| limit     | int      | Limit the number of items in the response to the requested number. |
| offset    | int      | Offset the items by the requested number. Use in conjunction with `limit` to support pagination. |

## MeetingsData

```json
{
  "meetings": [
    {...},
    {...}
  ]
}
```

| Name     | Type                    | Description        |
| -------- | ----------------------- | ------------------ |
| meetings | \[\][Meeting](#meeting) | Array of Meetings. |

## Meeting

```json
{
  "meeting": "",
  "name": "",
  "date": "YYYY-MM-DDT00:00:00Z",
  "category": "",
  "category_name": "",
  "country": "",
  "state": "",
  "races": [
    {...},
    {...}
  ]
}
```

| Name          | Type                            | Description                                                  |
| ------------- | ------------------------------- | ------------------------------------------------------------ |
| meeting       | string                          | UUIDv4 id of the meeting.                                    |
| name          | string                          | The name of the meeting location.                            |
| date          | date                            | The date that the meeting occurs on.                         |
| category      | string                          | One of `T`, `G`, or `H`.                                     |
| category_name | string                          | One of `Thoroughbred Horse Racing`, `Greyhound Racing` or `Harness Horse Racing`. |
| country       | string                          | The country that the meeting occurs in, if available.        |
| State         | string                          | The state that the meeting occurs in within the country, if available. |
| Races         | \[\][MeetingRace](#meetingrace) | Array of Races.                                              |

## MeetingRace

```json
{
  "id": "",
  "race_number": 0,
  "name": "",
  "start_time": "",
  "track_condition": "",
  "distance": 0,
  "weather": "",
  "country": "",
  "state": ""
}
```

| Name            | Type     | Description                                                  |
| --------------- | -------- | ------------------------------------------------------------ |
| id              | string   | UUIDv4 id of the race.                                       |
| race_number     | int      | The race number within the meeting. `0` is used for special races. |
| name            | string   | The name of the race.                                        |
| start_time      | datetime | The scheduled start time of the race. In [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format. |
| track_condition | string   | The condition of the track, if available.                    |
| distance        | int      | The distance of the track, if available.                     |
| weather         | string   | The weather at the track, if available.                      |
| country         | string   | The country that the meeting occurs in, if available.        |
| state           | string   | The state that the meeting occurs in within the country, if available. |

## AllMeetingsAndRacesParams

```json
{
  "enc": "json",
  "limit": 200,
  "offset": 0,
  "country": " ",
  "type": " ",
  "date_from": "2021-09-14T17:01:26.917532+10:00",
  "date_to": "2021-09-14T17:01:26.917534+10:00"
}
```

Parameter | Default | Choices | Description
--------- | ------- | -----------|--------- 
`enc` | `json` | `json`,`xml`,`html`,`help` | The response encoding format. 
`date_from` | `now` | | See [date formats](#date-fields). 
`date_to` | `now` | | See [date formats](#date-fields). 
`category` |  | `T`,`H`,`G` | Allows limiting the available **meetings** to one of Thoroughbred, Harness or Greyhound. 
`country` |  | `AUS`,`ARG`,`CA`,`CHI`,`DEU`,`FR`,... | Allows limiting the available **meetings** to only the requested country. See the `help` encoding for a full list. 
`limit` | `200` |  | Limit the number of **meetings** fetched. Max of 200. This limit will not reflect the size of the array as meetings have various amounts of races.
`offset` | `0` |  | Skip the first `offset` responses for **meetings**. 

## AllMeetingsAndRacesResponse

```json
{
  "header": {...},
  "params": {...},
  "data": {
    "meetings_races": [...]
  }
}
```

| Name   | Type                                      | Description                          |
| ------ | ----------------------------------------- | ------------------------------------ |
| header | [ResponseHeader](#responseheader)         | Meta data describing the response.   |
| params | [AllMeetingsAndRacesParams](#allmeetingsandracesparams) | The parameters sent in this request. |
| data   | [AllMeetingsAndRaces](#allmeetingsandraces)                   | All meetings and their race data.                      |

## AllMeetingsAndRaces

```json
{
  "meetings_races": [
    {...},
    {...},
  ],
}
```

| Name   | Type                                      | Description                          |
| ------ | ----------------------------------------- | ------------------------------------ |
| meetings_races |  \[\][MeetingAndAllRaceDetails](#meetingandallracedetails)        | An array of meetings and their respective race data.   |

## MeetingAndAllRaceDetails

```json
{
  "meeting": "",
  "name": "",
  "date": "YYYY-MM-DDT00:00:00Z",
  "category": "",
  "category_name": "",
  "country": "",
  "state": "",
  "races": [
    {...},
    {...}
  ]
}
```

| Name          | Type                            | Description                                                  |
| ------------- | ------------------------------- | ------------------------------------------------------------ |
| meeting       | string                          | UUIDv4 id of the meeting.                                    |
| name          | string                          | The name of the meeting location.                            |
| date          | date                            | The date that the meeting occurs on.                         |
| category      | string                          | One of `T`, `G`, or `H`.                                     |
| category_name | string                          | One of `Thoroughbred Horse Racing`, `Greyhound Racing` or `Harness Horse Racing`. |
| country       | string                          | The country that the meeting occurs in, if available.        |
| State         | string                          | The state that the meeting occurs in within the country, if available. |
| Races         | \[\][RacesData](#racesdata) | Array of race data.                                              |

## RacesResponse

```json
{
  "header": {...},
  "params": {...},
  "data": {...}
}
```

| Name   | Type                                      | Description                          |
| ------ | ----------------------------------------- | ------------------------------------ |
| header | [ResponseHeader](#responseheader)         | Meta data describing the response.   |
| params | [RacesRequestParams](#racesrequestparams) | The parameters sent in this request. |
| data   | [RacesData](#racesdata)                   | The races data.                      |

## RacesRequestParams

```json
{
  "enc": "json",
  "id": ""
}
```

See [Race Request Parameters](#race-request-parameters).

| Name | Type   | Description                           |
| ---- | ------ | ------------------------------------- |
| enc  | string | The encoding format for the response. |
| id   | string | UUIDv4 of the race requested.         |

## RacesData

```json
{
  "race": {...},
  "results": [
    {...},
    {...}
  ],
  "dividends": [
    {...},
    {...}
  ],
  "favourite": {...},
  "runners": [
    {...},
    {...}
  ],
  "derivatives": null,
  "mover": {...}
}
```

| Name        | Type                              | Description                                               |
| ----------- | --------------------------------- | --------------------------------------------------------- |
| race        | [Race](race)                      | The race data.                                            |
| results     | \[\][RaceResult](#raceresult)     | The race results, if available. Note: omitted if empty.   |
| dividends   | \[\][RaceDividend](#racedividend) | The race dividends, if available. Note: omitted if emtpy. |
| favourite   | [RaceEntrant](#raceentrant)       | The favourite entrant in the race.                        |
| runners     | \[\][RaceRunner](#racerunner)     | The runners in the race.                                  |
| derivatives | \[\][RaceRunner](#racerunner)     | The derivative runners in the race.                       |
| mover       | [RaceEntrant](#raceentrant)       | The market mover in the race.                             |

## Race

```json
{
  "event_id": "97df766c-cdde-4aeb-b72a-95776d2e59b0",
  "meeting_name": "Orange",
  "meeting_id": "4cdf2c76-229d-4cb8-a930-e3a2fe173b62",
  "status": "Final",
  "description": "Epiroc Mdn Plate",
  "advertised_start": 1607915100,
  "actual_start": 1607915100,
  "advertised_start_string": "2020-12-14 03:05:00 +0000 UTC",
  "actual_start_string": "2020-12-14 03:05:00 +0000 UTC",
  "race_number": 1,
  "type": "T",
  "country": "AUS",
  "state": "NSW",
  "distance": 1400,
  "weather": "Overcast",
  "form_guide": "",
  "comment": "ZOUNATE (5) resumes from a spell. He's been an enigma with eight placings from eleven starts. He's been competitive at a much higher level. Two trials back have been solid and surely today is he day. DAWN FLIGHT (9) had his chance when fourth here on debut behind Grazie. She's sure to improve from the race experience and should be kept safe. HOT BAHAMA (2) is making his stable debut for James Ponsonby. Hard to get a gauge on his two runs in Queensland but he must be respected. Blinkers are on. TWICE ON SUNDAY (11) best of the rest.",
  "silk_base_url": "drr38safykj6s.cloudfront.net",
  "track_condition": "Good3",
  "silk_url": "drr38safykj6s.cloudfront.net/182561_1_SPRITE_64x64.png"
}
```

| Name                    | Type   | Description                                                  |
| ----------------------- | ------ | ------------------------------------------------------------ |
| event_id                | string | UUIDv4 of the race.                                          |
| meeting_name            | string | The name of the meeting location.                            |
| meeting_id              | string | UUIDv4 of the meeting this race belongs to.                  |
| status                  | string | The status of the race. One of: `Open`, `Closed`, `Abandoned`, `Live`, `Interim`, `Final`. |
| description             | string | The name of the race.                                        |
| advertised_start        | int    | The [Unix Time](https://en.wikipedia.org/wiki/Unix_time) that the race is scheduled to start. |
| actual_start            | int    | The [Unix Time](https://en.wikipedia.org/wiki/Unix_time) that the race actually started. Note that this will be the same as the `advertised_start` until the race has started. |
| advertised_start_string | string | A textual representation of the `advertised_start` in the `YYYY-MM-DD HH:MM:SS +hh:mm TZD` format. |
| actual_start_string     | string | A textual representation of the `actual_start` in the `YYYY-MM-DD HH:MM:SS +hh:mm TZD` format. |
| race_number             | int    | The number of the race within the meeting. Note that `0` is used for special races within a meeting. |
| type                    | string | One of `T` (Thoroughbred), `G` (Greyhound) or `H` (Harness). |
| country                 | string | The country this race is in, if available.                   |
| state                   | string | The state within the country this race is in, if available.  |
| distance                | int    | The distance of the race in metres, if available.            |
| weather                 | string | The weather for this race, if available.                     |
| form_guide              | string | A description of the form guide for this race, if available. |
| comment                 | string | The racing comment, if available.                            |
| silk_base_url           | string | The base URL to use for generating silks, if available.      |
| track_condition         | string | The track condition for this race, if available.             |
| silk_url                | string | The image URL containing 64x64 silks ordered by the runner number, if available. |

## RaceResult

```json
{
  "position": 2,
  "name": "Dawn Flight",
  "barrier": 5,
  "runner_number": 9,
  "margin_length": 0.37
}
```

| Name          | Type   | Description                                                  |
| ------------- | ------ | ------------------------------------------------------------ |
| position      | int    | The position this entrant came in. Typically up to the first `4` positions are given, if available. |
| name          | string | The name of the entrant in this position.                    |
| barrier       | int    | The barrier the entrant ran from.                            |
| runner_number | int    | The number/saddle/box of the entrant in this position.       |
| margin_length | float  | How many lengths behind first place this position came, if available. |

## RaceDividend

```json
{
  "id": "3c16a3c1-3b29-4962-aad5-942b41cfb812",
  "tote": "QLD",
  "product_name": "Exacta",
  "status": "Final",
  "dividend": 37,
  "pool_size": 1210.6,
  "jackpot_size": 0,
  "positions": [
    {
      "runner_number": 2,
      "position": 1
    },
    {
      "runner_number": 9,
      "position": 2
    }
  ]
}
```

| Name         | Type                              | Description                                                  |
| ------------ | --------------------------------- | ------------------------------------------------------------ |
| id           | string                            | UUIDv4 of the dividend.                                      |
| tote         | string                            | Which tote this dividend belongs to (e.g. `VIC`, `NSW`, `QLD`, `UNKNOWN`). |
| product_name | string                            | Which product this dividend belongs to.                      |
| status       | string                            | The status of this dividend. One of `Interim` or `Final`.    |
| dividend     | float                             | The value of this dividend.                                  |
| pool_size    | float                             | The size of the pool, if available.                          |
| jackpot_size | float                             | The size of the jackpot, if available.                       |
| positions    | \[\][RacePosition](#raceposition) | The positions making up this dividend.                       |

## RacePosition

```json
{
  "runner_number": 4,
  "position": 1
}
```

| Name          | Type | Description                                            |
| ------------- | ---- | ------------------------------------------------------ |
| runner_number | int  | The number/saddle/box of the entrant in this position. |
| position      | int  | The position that this entrant came.                   |

## RaceEntrant

```json
{
  "market_name": "Final Field",
  "primary_market": true,
  "name": "Halvoya",
  "is_scratched": true,
  "scratch_time": 1607904547,
  "barrier": 6,
  "runner_number": 3,
  "prize_money": "2380",
  "age": 3,
  "sex": "F",
  "colour": "ch",
  "silk_colours": "Red, White And Black Diagonal Stripes, White Cap",
  "form_comment": "",
  "class_level": "",
  "jockey": "Will Price",
  "country": "AUS",
  "trainer_name": "R \u0026 M Griffiths \u0026 De Kock",
  "weight": {
    "allocated": "56",
    "total": "56"
  },
  "favourite": false,
  "mover": false
}
```



| Name           | Type                          | Description                                                  |
| -------------- | ----------------------------- | ------------------------------------------------------------ |
| entrant_id     | string                        | UUIDv4 of the entrant.                                       |
| market_name    | string                        | The name of the market this entrant belongs to. This will usually be `Final Field` for the primary market of the race. |
| primary_market | boolean                       | Whether this entrant is part of the primary market of the race. |
| name           | string                        | The name of this entrant.                                    |
| is_scratched   | boolean                       | Whether this entrant has been scratched.                     |
| barrier        | int                           | Which barrier/box this entrant runs from.                    |
| runner_number  | int                           | The runner/saddle/box number of this entrant.                |
| prize_money    | string                        | The life time prize money this entrant has earned, if available. |
| age            | int                           | The age of this entrant, if available.                       |
| sex            | string                        | The sex of this entrant, if available.                       |
| colour         | string                        | The colour of this entrant, if available.                    |
| silk_colours   | string                        | The colours of the silk this entrant is wearing, if available. |
| form_comment   | string                        | A comment describing this entrants form, if available.       |
| class_level    | string                        | The class level of this entrant, if available.               |
| jockey         | string                        | The jockey riding this entrant, if available.                |
| country        | string                        | The country of origin of this entrant, if available.         |
| trainer_name   | string                        | The trainer name of this entrant, if available.              |
| weight         | [RunnerWeight](#runnerweight) | The weight of this entrant, if available.                    |
| favourite      | boolean                       | Whether this entrant is the favourite.                       |
| mover          | boolean                       | Whether this entrant is the market mover.                    |

## RunnerWeight

```json
{
  "allocated": "56",
  "total": "56"
}
```

| Name      | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| allocated | string | The allocated weight of this runner, if available. |
| total     | string | The total weight of this runner, if available.     |

## RaceRunner

```json
{
  "market_name": "Final Field",
  "primary_market": true,
  "name": "Halvoya",
  "is_scratched": true,
  "scratch_time": 1607904547,
  "barrier": 6,
  "runner_number": 3,
  "prize_money": "2380",
  "age": 3,
  "sex": "F",
  "colour": "ch",
  "silk_colours": "Red, White And Black Diagonal Stripes, White Cap",
  "form_comment": "",
  "class_level": "",
  "jockey": "Will Price",
  "country": "AUS",
  "trainer_name": "R \u0026 M Griffiths \u0026 De Kock",
  "weight": {
    "allocated": "56",
    "total": "56"
  },
  "favourite": false,
  "mover": false,
  "meta": null,
  "flucs": [
    9.5
  ],
  "odds": {
    "fixed_win": 9.5,
    "fixed_place": 2.25
  },
  "scr_time": "2020-12-14T00:09:07Z",
  "competitor_id": "",
  "ride_guide_exists": false,
  "ride_guide_thumbnail": "",
  "ride_guide_file": "",
  "trainer": ""
}
```

**Note:** A RaceRunner expands on the [RaceEntrant](#raceentrant) fields, meaning that all fields in the *RaceEntrant* schema are part of the *RaceRunner* schema, along with the following fields.

| Name                 | Type          | Description                                                  |
| -------------------- | ------------- | ------------------------------------------------------------ |
| meta                 | object        | A key-value (string-string) object of additional metadata relating to the runner, if available. |
| flucs                | \[\]float     | An ordered array of Fixed Win price fluctuations this runner has had, if available. |
| odds                 | [Odds](#odds) | The current odds of this runner.                             |
| scr_time             | string        | The date and time the runner was scratched. In [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format. |
| competitor_id        | string        | Unused.                                                      |
| ride_guide_exists    | bool          | Whether a ride guide exists.                                 |
| ride_guide_thumbnail | string        | The thumbnail URL for the ride guide, if available.          |
| ride_guide_file      | string        | The URL to the ride guide, if available.                     |
| trainer              | string        | Unused. See `trainer_name`.                                  |

## Odds

```json
{
  "fixed_win": 9.5,
  "fixed_place": 2.25
}
```

| Name        | Type  | Description                          |
| ----------- | ----- | ------------------------------------ |
| fixed_win   | float | The fixed win price.                 |
| fixed_place | float | The fixed place price, if available. |

