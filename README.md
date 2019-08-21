# Documentation for Affiliate Feed services (public)

### Status 21 AUG 2019
- [x] Overview of the structure of the new API
- [x] Overview of working endpoints in new API

TODO
- [ ] Details of Racing API
- [ ] Details of Sports API
- [ ] Details of Racing Resulting
- [ ] Details of Sports Resulting
- [ ] BetNow Functionality

## Scope

This documentation is provided for external developers who need to write code to poll data from the provided 
sites.

The API itself is designed to be consistent across all endpoints, and provide some built in tools to make
connecting to the API, debugging and maintaining your code that little bit easier.

# API Endpoints - Multiple Brands

All Brands follow the same endpoint syntax, and provide identical output formats.

The base URL for each brand is as follows: 

- Ladbrokes https://api-affiliates.ladbrokes.com.au
- Neds https://api-affiliates.neds.com.au
- BetStar https://api-affiliates.betstar.com.au
- BookMaker https://api-affiliates.bookmaker.com.au

In the document below, we will refer to this as `BASE_URL`

The API provided on the `BASE_URL` uses `HTTP` protocol - so can be hit from a browser, curl script, or coded using
standard HTTP libs in your prefered development toolkit.

Each API endpoint has a consistent range of `GET` parameters for controlling the returned data.

# UUID format ID Fields

Note that all ID fields across all APIs now use UUIDv4 notation instead of simple integers that
may have been used previously.

MeetingIDs / EventIDs are consistent across all brands, so they can be re-used across 
different `BASE_URL`s to refer the exact same Meeting / Event / Entrant, from one brand to the next.

eg

- https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4
- https://api-affiliates.neds.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4
- https://api-affiliates.betstar.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4
- https://api-affiliates.bookmaker.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4

Are 4 different views of the same event, with the pricing data applicable to that brand only.

# Data vs Index API Endpoints

There are 2 classes of API Endpoint provided.

- Index API
- Data API

Index APIs provide HTML formatted index pages for basic queries.
  
These are provided for the developer as an aide for debugging and setting up your application code.

For example, the `BASE_URL` returns a HTML formatted page with a human readable index of what URLs
are available, and how to invoke them.

eg. https://api-affiliates.ladbrokes.com.au gives us this Index, with an example URL in each case,
and a listing of which parameters are accepted.
![baseurl](screenshots/1.png)

### Data API endpoints and Encoding types

`JSON` Is the default encoding for the payload on Data APIs

- https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4 

returns a JSON payload with the data for the given racing event.

---

`XML` Options - append `.xml` to the URL

- https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.xml

returns an XML payload for the exact same data.

---

`HTML` Options to get a human readable index page of the exact same data, for easy debugging. 

- https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.html

---

Optional - using the `enc` GET parameter to control output encoding format

- `?enc=html`  Returns `HTML` format 
- `?enc=json`  Returns `JSON` format
- `?enc=xml` Returns `XML` format.  (not available on all endpoints)
- `?enc=help` Returns a short description of the available parameters. (Debugging aide)

This is useful, as it can be applied to Index URLs where adding a `.html` is not applicable, such as 

- `JSON` https://api-affiliates.ladbrokes.com.au/racing/meetings for a JSON data payload, vs
- `XML` https://api-affiliates.ladbrokes.com.au/racing/meetings?enc=xml for an XML data payload, vs
- `HTML` https://api-affiliates.ladbrokes.com.au/racing/meetings?enc=html for a human readable view of the same data
- `PARAMETER HELP` https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4?enc=help

---


# Racing APIs

Summary of API endpoints for racing
- `BASE_URL`/meetings
- `BASE_URL`/meetings/`MEETING-ID`
- `BASE_URL`/events/`EVENT-ID`

### Racing `BASE_URL`/meetings

- `JSON` https://api-affiliates.ladbrokes.com.au/racing/meetings
- `HTML` https://api-affiliates.ladbrokes.com.au/racing/meetings?enc=html
- `XML` https://api-affiliates.ladbrokes.com.au/racing/meetings?enc=xml

Parameters
```cassandraql
Param: enc             Default: json  Choices: json,xml,html
Param: id              
Param: date_from       Default: week
Param: date_to         Default: now
Param: category        Default:    Choices: T,H,G, 
Param: country         Default:    Choices:  ,AUS,ARG,CA,CHI,DEU,FR,HK,IND,IRE,JPN,KOR,NOR,NZ,PER,SAF,SGP,SWE,UK,URY,USA
Param: limit           Default: 20
```

### Racing `BASE_URL`/meetings/`MEETING-ID`

- `JSON` https://api-affiliates.ladbrokes.com.au/racing/meetings/1f2d507d-e4b7-4d14-8463-17842301684b
- `XML` https://api-affiliates.ladbrokes.com.au/racing/meetings/1f2d507d-e4b7-4d14-8463-17842301684b.xml
- `HTML` https://api-affiliates.ladbrokes.com.au/racing/meetings/1f2d507d-e4b7-4d14-8463-17842301684b.html

Parameters
```cassandraql
Param: enc             Default: json  Choices: json,xml,html
Param: id              
Param: date_from       Default: week
Param: date_to         Default: now
Param: category        Default:    Choices: T,H,G, 
Param: country         Default:    Choices:  ,AUS,ARG,CA,CHI,DEU,FR,HK,IND,IRE,JPN,KOR,NOR,NZ,PER,SAF,SGP,SWE,UK,URY,USA
Param: limit           Default: 20
```

### Racing `BASE_URL`/events/`EVENT-ID`

- `JSON` https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4
- `XML` https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.xml
- `HTML` https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.html

Parameters
```cassandraql
Param: enc             Default: json  Choices: json,xml,html
Param: id              
Param: limit           Default: 100
Param: offset          Default: 0
Param: country         Default:    Choices: AU, ,SAF,UK,NZ,IN,KR,JP
Param: type            Default: T  Choices: T,H,G
Param: from_date       
Param: to_date         
```

# Sports API

WorkInProgress here.

The following denotes the proposed API to be delivered. This is mostly a mirror of the Racing API, with 
the additional split by competitions, and slightly different search parameters.

The Links below are examples only, and will not work till the new Sport API is rolled out.

Summary of API endpoints for sports
- `BASE_URL`/competitions
- `BASE_URL`/competitions/`COMPETITON-ID`
- `BASE_URL`/events/`EVENT-ID`

### Sports `BASE_URL`/competitions

- `JSON` https://api-affiliates.ladbrokes.com.au/sports/competitions
- `HTML` https://api-affiliates.ladbrokes.com.au/sports/competitions?enc=html
- `XML` https://api-affiliates.ladbrokes.com.au/sports/competitions?enc=xml

### Sports `BASE_URL`/competitions/`MEETING-ID`

- `JSON` https://api-affiliates.ladbrokes.com.au/sports/competitions/1f2d507d-e4b7-4d14-8463-17842301684b
- `XML` https://api-affiliates.ladbrokes.com.au/sports/competitions/1f2d507d-e4b7-4d14-8463-17842301684b.xml
- `HTML` https://api-affiliates.ladbrokes.com.au/sports/competitions/1f2d507d-e4b7-4d14-8463-17842301684b.html

### Sports `BASE_URL`/events/`EVENT-ID`

- `JSON` https://api-affiliates.ladbrokes.com.au/sports/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4
- `XML` https://api-affiliates.ladbrokes.com.au/sports/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.xml
- `HTML` https://api-affiliates.ladbrokes.com.au/sports/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.html

