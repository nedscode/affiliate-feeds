---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - go

toc_footers:
  - <a href='https://github.com/nedscode/affiliate-feeds/issues'>Submit an Issue</a>
  - <a href='https://www.ladbrokes.com.au/contact-us'>Contact Us</a>

includes:
  - deeplinks
  - schemas
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the Ladbrokes Affiliate API!

The base URL is `https://api-affiliates.ladbrokes.com.au/affiliates/v1/` and further documentation may refer to this as `BASE_URL`.

Dates & times are returned in UTC using [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) unless otherwise specified.

## Identification

> Please include identifying Headers with all of your requests:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "From: joe@blogs.com.au" \
  -H "X-Partner: Joe Blogs Pty Ltd"
```

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"time"
)

func main() {
	// Always use a http client with a timeout
	client := &http.Client{
		Timeout: time.Second * 30,
	}

	// Set an appropriate API endpoint URL
	req, err := http.NewRequest("GET", "api_endpoint_here", nil)
	if err != nil {
		panic(err)
	}

	// Remember to set your identifying values to avoid being blocked
	req.Header.Add("From", "joe@blogs.com.au")
	req.Header.Add("X-Partner", "Joe Blogs Pty Ltd")

	resp, err := client.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		panic(err)
	}

	fmt.Println(string(body))
}

```

> Make sure to provide values that identify you otherwise you may end up being rate limited, or blocked.

To avoid being blocked or inappropriately rate limited, please include Headers to identify who you are and provide a way to contact you if needed:

`From: joe@blogs.com.au`

`X-Partner: Joe Blogs Pty Ltd`

<aside class="notice">
You must replace the values of the Headers with values that accurately describe who you are and how we can contact you if needed.
</aside>
## ID Fields

All ID fields that are UUIDs make use of the [Version 4 format](https://en.wikipedia.org/wiki/Universally_unique_identifier#Version_4_(random)) and conform to [RFC 4122](https://www.ietf.org/rfc/rfc4122.txt).

# Response Formats

By default, responses are sent in `JSON` format. You may also request data formatted as `XML` or `HTML`.

You can use the `enc` GET parameter to control the encoding format.

## JSON Format

The default for data payloads.

`?enc=json`

## XML Format

Not all endpoints support XML.

`?enc=xml`

## HTML Format

Useful mainly for debugging/navigating.

`?enc=html`

## Help Format

Returns a short description of the available parameters for the requested endpoint.

`?enc=help`

# Request Parameter Formats

## Date Fields

Common parameter names:

* `date_from`
* `date_to`

Dates accept strings in the format `YYYY-MM-DD` as well as several keywords:

| Keyword | Description                                                  |
| ------- | ------------------------------------------------------------ |
| `now`   | The current date and time                                    |
| `today` | The current day                                              |
| `week`  | The start of the current week (where Monday is the start of the week) |
| `month` | The start of the current month                               |
| `all`   | All available dates                                          |

Date ranges can be considered inclusive.

## Parameters with Choices

> Example response from a `?enc=help` request.

```shell
Param: category        Default:    Choices: T,H,G, 

```

```go
Param: category        Default:    Choices: T,H,G, 

```

> The value of `?category=VALUE` must be one of `T`, `H`, or `G` for the request to be valid.

If a parameter has a `Choices` set (as displayed in the `?enc=help` response for that endpoint), then the input data must be one of the values shown.

# Racing APIs

Available endpoints:

* [`BASE_URL`/meetings](#racing-meetings)
* [`BASE_URL`/meetings/`MEETING_ID`](#get-a-specific-meeting)
* [`BASE_URL`/events/`EVENT_ID`](#get-a-specific-race)

## Racing Meetings


```shell
curl "https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/meetings" \
  -H "From: joe@blogs.com.au" \
  -H "X-Partner: Joe Blogs Pty Ltd"
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
	"time"
)

type Response struct {
	Header     ResponseHeader `json:"header" xml:"header"`
	Parameters RequestParams  `json:"params" xml:"params"`
	Data       Data           `json:"data" xml:"data"`
}

type ResponseHeader struct {
	Title         string    `json:"title" xml:"title"`
	GeneratedTime time.Time `json:"generated_time" xml:"generated_time"`
	URL           string    `json:"url" xml:"url"`
	Error         string    `json:"error,omitempty" xml:"error,omitempty"`
}

type RequestParams struct {
	Encoding string    `json:"enc" xml:"Encoding"`
	ID       string    `json:"id" xml:"MeetingID"`
	DateFrom time.Time `json:"date_from" xml:"FromDate"`
	DateTo   time.Time `json:"date_to" xml:"ToDate"`
	Category string    `json:"type" xml:"Category"`
	Country  string    `json:"country" xml:"Country"`
	Limit    int       `json:"limit" xml:"Limit"`
	Offset   int       `json:"offset" xml:"Offset"`
}

type Data struct {
	Meetings []*Meeting `json:"meetings" xml:"meetings"`
}

type Meeting struct {
	Meeting      string    `json:"meeting" xml:"meeting"`
	Name         string    `json:"name" xml:"name"`
	Date         time.Time `json:"date" xml:"date"`
	Category     string    `json:"category" xml:"category"`
	CategoryName string    `json:"category_name" xml:"category_name"`
	Country      string    `json:"country" xml:"country"`
	State        string    `json:"state" xml:"state"`
	Races        []*Race   `json:"races" xml:"races"`
}

type Race struct {
	ID             string    `json:"id" xml:"id"`
	RaceNumber     uint32    `json:"race_number" xml:"race_number"`
	Name           string    `json:"name" xml:"name"`
	StartTime      time.Time `json:"start_time" xml:"start_time"`
	TrackCondition string    `json:"track_condition" xml:"track_condition"`
	Distance       uint32    `json:"distance" xml:"distance"`
	Weather        string    `json:"weather" xml:"weather"`
	Country        string    `json:"country" xml:"country"`
	State          string    `json:"state" xml:"state"`
}

func main() {
	// Always use a http client with a timeout
	client := &http.Client{
		Timeout: time.Second * 30,
	}

	req, err := http.NewRequest("GET", "https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/meetings", nil)
	if err != nil {
		panic(err)
	}

	// Remember to set your identifying values to avoid being blocked
	req.Header.Add("From", "joe@blogs.com.au")
	req.Header.Add("X-Partner", "Joe Blogs Pty Ltd")

	r, err := client.Do(req)
	if err != nil {
		panic(err)
	}
	defer r.Body.Close()

	var response Response
	err = json.NewDecoder(r.Body).Decode(&response)
	if err != nil {
		panic(err)
	}

	fmt.Printf("%s Retrieved %d meetings.", response.Header.GeneratedTime, len(response.Data.Meetings))
}

```

> Example JSON response:

```json
{
  "header": {
    "title": "Race Meetings",
    "generated_time": "2020-12-10T08:19:47.069943716Z",
    "url": "/affiliates/v1/racing/meetings"
  },
  "params": {
    "enc": "json",
    "id": "",
    "date_from": "2020-12-10T08:19:47.06993608Z",
    "date_to": "2020-12-10T08:19:47.069938012Z",
    "type": " ",
    "country": " ",
    "limit": 100,
    "offset": 0
  },
  "data": {
    "meetings": [
      {
        "meeting": "7e09a407-d61e-407b-94b3-fe5d293cbfde",
        "name": "Queanbeyan",
        "date": "2020-12-10T00:00:00Z",
        "category": "T",
        "category_name": "Thoroughbred Horse Racing",
        "country": "AUS",
        "state": "NSW",
        "races": [
          {
            "id": "2b936876-7524-40f5-be6a-3b1d8cc844af",
            "race_number": 0,
            "name": "Jockey Challenge - Queanbeyan",
            "start_time": "2020-12-10T02:15:00Z",
            "track_condition": "",
            "distance": 0,
            "weather": "",
            "country": "",
            "state": ""
          },
          {
            "id": "7ab0527f-0746-415b-82cd-968e14629aaa",
            "race_number": 1,
            "name": "Skyview Windows Mdn Plate",
            "start_time": "2020-12-10T02:15:00Z",
            "track_condition": "",
            "distance": 0,
            "weather": "",
            "country": "",
            "state": ""
          }
        ]
      }
    ]
  }
}
```

This endpoint retrieves all meetings.

### HTTP Request

`GET https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/meetings`

### Meeting Request Parameters

Parameter | Default | Choices | Description
--------- | ------- | -----------|--------- 
`enc` | `json` | `json`,`xml`,`html`,`help` | The response encoding format. 
`id` |  |  | If provided, get the requested meeting by its ID. 
`date_from` | `week` | | See [date formats](#date-fields). 
`date_to` | `now` | | See [date formats](#date-fields). 
`category` |  | `T`,`H`,`G` | Allows limiting the response to one of Thoroughbred, Harness or Greyhound meetings. 
`country` |  | `AUS`,`ARG`,`CA`,`CHI`,`DEU`,`FR`,... | Allows limiting the response to only the requested country. See the `help` encoding for a full list. 
`limit` | `100` |  | Limit the number of items included in the response. Max of 200. 
`offset` | `0` |  | Skip the first `offset` responses. 

### Response Schema

[MeetingsResponse](#meetingsresponse)

<aside class="success">
Remember — Please include your identification headers!
</aside>


## Get a Specific Meeting


```shell
curl "https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/meetings/1f2d507d-e4b7-4d14-8463-17842301684b" \
  -H "From: joe@blogs.com.au" \
  -H "X-Partner: Joe Blogs Pty Ltd"
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
	"time"
)

type Response struct {
	Header     ResponseHeader `json:"header" xml:"header"`
	Parameters RequestParams  `json:"params" xml:"params"`
	Data       Data           `json:"data" xml:"data"`
}

type ResponseHeader struct {
	Title         string    `json:"title" xml:"title"`
	GeneratedTime time.Time `json:"generated_time" xml:"generated_time"`
	URL           string    `json:"url" xml:"url"`
	Error         string    `json:"error,omitempty" xml:"error,omitempty"`
}

type RequestParams struct {
	Encoding string    `json:"enc" xml:"Encoding"`
	ID       string    `json:"id" xml:"MeetingID"`
	DateFrom time.Time `json:"date_from" xml:"FromDate"`
	DateTo   time.Time `json:"date_to" xml:"ToDate"`
	Category string    `json:"type" xml:"Category"`
	Country  string    `json:"country" xml:"Country"`
	Limit    int       `json:"limit" xml:"Limit"`
	Offset   int       `json:"offset" xml:"Offset"`
}

type Data struct {
	Meetings []*Meeting `json:"meetings" xml:"meetings"`
}

type Meeting struct {
	Meeting      string    `json:"meeting" xml:"meeting"`
	Name         string    `json:"name" xml:"name"`
	Date         time.Time `json:"date" xml:"date"`
	Category     string    `json:"category" xml:"category"`
	CategoryName string    `json:"category_name" xml:"category_name"`
	Country      string    `json:"country" xml:"country"`
	State        string    `json:"state" xml:"state"`
	Races        []*Race   `json:"races" xml:"races"`
}

type Race struct {
	ID             string    `json:"id" xml:"id"`
	RaceNumber     uint32    `json:"race_number" xml:"race_number"`
	Name           string    `json:"name" xml:"name"`
	StartTime      time.Time `json:"start_time" xml:"start_time"`
	TrackCondition string    `json:"track_condition" xml:"track_condition"`
	Distance       uint32    `json:"distance" xml:"distance"`
	Weather        string    `json:"weather" xml:"weather"`
	Country        string    `json:"country" xml:"country"`
	State          string    `json:"state" xml:"state"`
}

func main() {
	// Always use a http client with a timeout
	client := &http.Client{
		Timeout: time.Second * 30,
	}

	req, err := http.NewRequest("GET", "https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/meetings/1f2d507d-e4b7-4d14-8463-17842301684b", nil)
	if err != nil {
		panic(err)
	}

	// Remember to set your identifying values to avoid being blocked
	req.Header.Add("From", "joe@blogs.com.au")
	req.Header.Add("X-Partner", "Joe Blogs Pty Ltd")

	r, err := client.Do(req)
	if err != nil {
		panic(err)
	}
	defer r.Body.Close()

	var response Response
	err = json.NewDecoder(r.Body).Decode(&response)
	if err != nil {
		panic(err)
	}

	fmt.Printf("%s Retrieved %d meetings.", response.Header.GeneratedTime, len(response.Data.Meetings))
}

```

> Example JSON response:

```json
{
  "header": {
    "title": "Race Meetings",
    "generated_time": "2020-12-10T08:19:47.069943716Z",
    "url": "/affiliates/v1/racing/meetings"
  },
  "params": {
    "enc": "json",
    "id": "",
    "date_from": "2020-12-10T08:19:47.06993608Z",
    "date_to": "2020-12-10T08:19:47.069938012Z",
    "type": " ",
    "country": " ",
    "limit": 100,
    "offset": 0
  },
  "data": {
    "meetings": [
      {
        "meeting": "7e09a407-d61e-407b-94b3-fe5d293cbfde",
        "name": "Queanbeyan",
        "date": "2020-12-10T00:00:00Z",
        "category": "T",
        "category_name": "Thoroughbred Horse Racing",
        "country": "AUS",
        "state": "NSW",
        "races": [
          {
            "id": "2b936876-7524-40f5-be6a-3b1d8cc844af",
            "race_number": 0,
            "name": "Jockey Challenge - Queanbeyan",
            "start_time": "2020-12-10T02:15:00Z",
            "track_condition": "",
            "distance": 0,
            "weather": "",
            "country": "",
            "state": ""
          },
          {
            "id": "7ab0527f-0746-415b-82cd-968e14629aaa",
            "race_number": 1,
            "name": "Skyview Windows Mdn Plate",
            "start_time": "2020-12-10T02:15:00Z",
            "track_condition": "",
            "distance": 0,
            "weather": "",
            "country": "",
            "state": ""
          }
        ]
      }
    ]
  }
}
```

This endpoint retrieves a specific meeting.

### HTTP Request

`GET https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/meetings/<ID>`

### Meeting Request Parameters

Parameter | Default | Choices | Description 
--------- | -----------|--------- |--------- 
`enc` | `json` | `json`,`xml`,`html`,`help` | The response encoding format. 
`id` |  | | The ID of the meeting to retrieve. 

### Response Schema

[MeetingsResponse](#meetingsresponse)

<aside class="success">
Remember — Please include your identification headers!
</aside>


## Get a Specific Race


```shell
curl "https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/events/ad76bf91-684d-4a49-bf04-b4284237e65d" \
  -H "From: joe@blogs.com.au" \
  -H "X-Partner: Joe Blogs Pty Ltd"
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
	"time"
)

type Response struct {
	Header     ResponseHeader `json:"header" xml:"header"`
	Parameters RequestParams  `json:"params" xml:"params"`
	Data       Data           `json:"data" xml:"data"`
}

type ResponseHeader struct {
	Title         string    `json:"title" xml:"title"`
	GeneratedTime time.Time `json:"generated_time" xml:"generated_time"`
	URL           string    `json:"url" xml:"url"`
	Error         string    `json:"error,omitempty" xml:"error,omitempty"`
}

type RequestParams struct {
	Encoding string    `json:"enc" xml:"Encoding"`
	ID       string    `json:"id" xml:"EventID"`
	DateFrom time.Time `json:"date_from" xml:"FromDate"`
	DateTo   time.Time `json:"date_to" xml:"ToDate"`
	Category string    `json:"type" xml:"Category"`
	Country  string    `json:"country" xml:"Country"`
	Limit    int       `json:"limit" xml:"Limit"`
	Offset   int       `json:"offset" xml:"Offset"`
}

type Data struct {
	Race        Race        `json:"race"`
	Results     []*Result   `json:"results,omitempty"`
	Dividends   []*Dividend `json:"dividends,omitempty"`
	Favourite   Entrant     `json:"favourite"`
	Runners     []*Runner   `json:"runners"`
	Derivatives []*Runner   `json:"derivatives"`
	Mover       Entrant     `json:"mover"`
	Error       string      `json:"error"`
}

type Race struct {
	EventID               string `json:"event_id"`
	MeetingName           string `json:"meeting_name"`
	MeetingID             string `json:"meeting_id"`
	Status                string `json:"status"`
	Description           string `json:"description"`
	AdvertisedStart       int64  `json:"advertised_start"`
	ActualStart           int64  `json:"actual_start"`
	AdvertisedStartString string `json:"advertised_start_string"`
	ActualStartString     string `json:"actual_start_string"`
	RaceNumber            uint32 `json:"race_number"`
	Type                  string `json:"type"`
	Country               string `json:"country"`
	State                 string `json:"state"`
	Distance              uint32 `json:"distance"`
	Weather               string `json:"weather"`
	FormGuide             string `json:"form_guide"`
	Comment               string `json:"comment"`
	SilkBaseURL           string `json:"silk_base_url"`
	TrackCondition        string `json:"track_condition"`
	SilkURL               string `json:"silk_url"`
}

type Result struct {
	Position     uint32  `json:"position"`
	Name         string  `json:"name"`
	Barrier      uint32  `json:"barrier"`
	RunnerNumber uint32  `json:"runner_number"`
	MarginLength float32 `json:"margin_length"`
}

type Dividend struct {
	ID          string      `json:"id"`
	Tote        string      `json:"tote"`
	ProductName string      `json:"product_name"`
	Status      string      `json:"status"`
	Dividend    float32     `json:"dividend"`
	PoolSize    float32     `json:"pool_size"`
	JackpotSize float32     `json:"jackpot_size"`
	Positions   []*Position `json:"positions"`
}

type Position struct {
	RunnerNumber uint32 `json:"runner_number"`
	Position     uint32 `json:"position"`
}

type Entrant struct {
	EntrantID     string `json:"-"`
	MarketName    string `json:"market_name"`
	PrimaryMarket bool   `json:"primary_market"`
	Name          string `json:"name"`
	IsScratched   bool   `json:"is_scratched"`
	ScratchTime   int64  `json:"scratch_time"`
	Barrier       uint32 `json:"barrier"`
	RunnerNumber  int    `json:"runner_number"`
	PrizeMoney    string `json:"prize_money"`
	Age           int    `json:"age"`
	Sex           string `json:"sex"`
	Colour        string `json:"colour"`
	SilkColours   string `json:"silk_colours"`
	FormComment   string `json:"form_comment"`
	ClassLevel    string `json:"class_level"`
	Jockey        string `json:"jockey"`
	Country       string `json:"country"`
	TrainerName   string `json:"trainer_name"`
	Weight        struct {
		Allocated string `json:"allocated"`
		Total     string `json:"total"`
	} `json:"weight"`
	Favourite bool `json:"favourite"`
	Mover     bool `json:"mover"`
}

type Runner struct {
	Entrant
	Meta               map[string]string `json:"meta" xml:"-"`
	Flucs              []float64         `json:"flucs" xml:"-"`
	Odds               Odds              `json:"odds"`
	ScrTime            *time.Time        `json:"scr_time"`
	CompetitorID       string            `json:"competitor_id"`
	RideGuideExists    bool              `json:"ride_guide_exists"`
	RideGuideThumbnail string            `json:"ride_guide_thumbnail"`
	RideGuideFile      string            `json:"ride_guide_file"`
	Trainer            string            `json:"trainer"`
}

type Odds struct {
	FixedWin   float64 `json:"fixed_win"`
	FixedPlace float64 `json:"fixed_place"`
}

func main() {
	// Always use a http client with a timeout
	client := &http.Client{
		Timeout: time.Second * 30,
	}

	req, err := http.NewRequest("GET", "https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/events/ad76bf91-684d-4a49-bf04-b4284237e65d", nil)
	if err != nil {
		panic(err)
	}

	// Remember to set your identifying values to avoid being blocked
	req.Header.Add("From", "joe@blogs.com.au")
	req.Header.Add("X-Partner", "Joe Blogs Pty Ltd")

	r, err := client.Do(req)
	if err != nil {
		panic(err)
	}
	defer r.Body.Close()

	var response Response
	err = json.NewDecoder(r.Body).Decode(&response)
	if err != nil {
		panic(err)
	}

	fmt.Printf("%s Retrieved %s", response.Header.GeneratedTime, response.Data.Race.Description)
}

```

> Example JSON response:

```json
{
  "header": {
    "title": "Event Details: ad76bf91-684d-4a49-bf04-b4284237e65d",
    "generated_time": "2020-12-10T08:44:53.024562314Z",
    "url": "/affiliates/v1/racing/events?id=ad76bf91-684d-4a49-bf04-b4284237e65d"
  },
  "params": {
    "enc": "json",
    "id": "ad76bf91-684d-4a49-bf04-b4284237e65d",
    "limit": 200,
    "offset": 0,
    "country": " ",
    "type": "T",
    "date_from": "0001-01-01T00:00:00Z",
    "date_to": "0001-01-01T00:00:00Z"
  },
  "data": {
    "race": {
      "event_id": "ad76bf91-684d-4a49-bf04-b4284237e65d",
      "meeting_name": "Palm Beach",
      "meeting_id": "1f2d507d-e4b7-4d14-8463-17842301684b",
      "status": "Final",
      "description": "Race 5",
      "advertised_start": 1566151560,
      "actual_start": 1566151560,
      "advertised_start_string": "2019-08-18 18:06:00 +0000 UTC",
      "actual_start_string": "2019-08-18 18:06:00 +0000 UTC",
      "race_number": 5,
      "type": "G",
      "country": "USA",
      "state": "FL",
      "distance": 0,
      "weather": "",
      "form_guide": "",
      "comment": "",
      "silk_base_url": "",
      "track_condition": ""
    },
    "results": [
      {
        "position": 1,
        "name": "Hot Dog Safari",
        "barrier": 4,
        "runner_number": 4,
        "margin_length": 0
      },
      {
        "position": 2,
        "name": "Megan Rapinoe",
        "barrier": 7,
        "runner_number": 7,
        "margin_length": 0
      },
      {
        "position": 3,
        "name": "Pj Smoke Out",
        "barrier": 3,
        "runner_number": 3,
        "margin_length": 0
      }
    ],
    "dividends": [
      {
        "id": "0fdd0e77-e219-4d01-8f21-dd17427f8af6",
        "tote": "UNKNOWN",
        "product_name": "Trifecta",
        "status": "Final",
        "dividend": 133.71,
        "pool_size": 0,
        "jackpot_size": 0,
        "positions": [
          {
            "runner_number": 4,
            "position": 1
          },
          {
            "runner_number": 7,
            "position": 2
          },
          {
            "runner_number": 3,
            "position": 3
          }
        ]
      },
      {
        "id": "25168e14-f1d6-4664-9ced-0233aa1df8d8",
        "tote": "UNKNOWN",
        "product_name": "Exacta",
        "status": "Final",
        "dividend": 30.5,
        "pool_size": 0,
        "jackpot_size": 0,
        "positions": [
          {
            "runner_number": 4,
            "position": 1
          },
          {
            "runner_number": 7,
            "position": 2
          }
        ]
      }
    ],
    "favourite": {
      "market_name": "Final Field",
      "primary_market": true,
      "name": "Atascocita Leadn",
      "is_scratched": false,
      "scratch_time": 0,
      "barrier": 6,
      "runner_number": 6,
      "prize_money": "",
      "age": 0,
      "sex": "",
      "colour": "",
      "silk_colours": "",
      "form_comment": "",
      "class_level": "",
      "jockey": "",
      "country": "",
      "trainer_name": "",
      "weight": {
        "allocated": "",
        "total": ""
      },
      "favourite": false,
      "mover": false
    },
    "runners": [
      {
        "market_name": "Final Field",
        "primary_market": true,
        "name": "Kissa",
        "is_scratched": false,
        "scratch_time": 0,
        "barrier": 1,
        "runner_number": 1,
        "prize_money": "",
        "age": 0,
        "sex": "",
        "colour": "",
        "silk_colours": "",
        "form_comment": "",
        "class_level": "",
        "jockey": "",
        "country": "",
        "trainer_name": "",
        "weight": {
          "allocated": "",
          "total": ""
        },
        "favourite": false,
        "mover": false,
        "meta": null,
        "flucs": [
          4.6,
          4.8
        ],
        "odds": {
          "fixed_win": 4.8,
          "fixed_place": 1.75
        },
        "scr_time": null,
        "competitor_id": "",
        "ride_guide_exists": false,
        "ride_guide_thumbnail": "",
        "ride_guide_file": "",
        "trainer": ""
      },
      {
        "market_name": "Final Field",
        "primary_market": true,
        "name": "Arkans Canberra",
        "is_scratched": false,
        "scratch_time": 0,
        "barrier": 2,
        "runner_number": 2,
        "prize_money": "",
        "age": 0,
        "sex": "",
        "colour": "",
        "silk_colours": "",
        "form_comment": "",
        "class_level": "",
        "jockey": "",
        "country": "",
        "trainer_name": "",
        "weight": {
          "allocated": "",
          "total": ""
        },
        "favourite": false,
        "mover": false,
        "meta": null,
        "flucs": [
          11,
          12
        ],
        "odds": {
          "fixed_win": 12,
          "fixed_place": 3.2
        },
        "scr_time": null,
        "competitor_id": "",
        "ride_guide_exists": false,
        "ride_guide_thumbnail": "",
        "ride_guide_file": "",
        "trainer": ""
      }
    ],
    "derivatives": null,
    "mover": {
      "market_name": "Final Field",
      "primary_market": true,
      "name": "Hot Dog Safari",
      "is_scratched": false,
      "scratch_time": 0,
      "barrier": 4,
      "runner_number": 4,
      "prize_money": "",
      "age": 0,
      "sex": "",
      "colour": "",
      "silk_colours": "",
      "form_comment": "",
      "class_level": "",
      "jockey": "",
      "country": "",
      "trainer_name": "",
      "weight": {
        "allocated": "",
        "total": ""
      },
      "favourite": false,
      "mover": false
    },
    "error": ""
  }
}
```

This endpoint retrieves a specific race.

### HTTP Request

`GET https://api-affiliates.ladbrokes.com.au/affiliates/v1/racing/events/{id}`

### Race Request Parameters

| Parameter | Default | Choices                    | Description                     |
| --------- | ------- | -------------------------- | ------------------------------- |
| `enc`     | `json`  | `json`,`xml`,`html`,`help` | The response encoding format.   |
| `id`      |         |                            | The ID of the race to retrieve. |

### Response Schema

[RacesResponse](#racesresponse)



<aside class="success">
Remember — Please include your identification headers!
</aside>

# Sports API

<aside class="warning">
The sports API is not available via the affiliates API.
</aside>

