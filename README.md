# Documentation for Affiliate Feed services (public)

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

returns a `JSON` payload with the data for the given racing event.

---

`XML` Options - append `.xml` to the URL

- https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.xml

returns an XML payload for the exact same data.

---

`HTML` Options to get a human readable index page of the exact same data, for easy debugging. 

- https://api-affiliates.ladbrokes.com.au/racing/events/d43cc25b-2b27-4d5d-818e-b2f8b40399e4.html


### Racing

#### 

###

### Optional - using the `enc` GET parameter to control output encoding format

- `?enc=html`  Returns `HTML` format (DEFAULT)
- `?enc=json`  Returns `JSON` format (Available for all data endpoints)
- `?enc=xml` Returns `XML` format.  (not available on all endpoints)
