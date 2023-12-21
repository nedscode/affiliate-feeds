# Deep Links

## Racing Bets
`Available for: iOS and Android Mobile Application`

`Available from: Version 8.69.0`

Currently we support adding bets within our platform from external sources via deep linking. URL format must follow the appropriate syntax to be recognised:

`/racing/<meetingName>/<raceId>/<entrantId>/<dividendCode>`

Where:

Parameter | Type | Description | Example
--------- | ----------- | --------- | ---------
`meetingName` | `string` | The name of the meeting (`name`). See [Meeting](#meeting). | `Geelong`
`raceId` | `string` | UUIDv4 representing the race (`event_id`). See [Race](#race). | `d3c7c5f8-5e4a-40e6-a1e7-69d6d9fd722d`
`entrantId` | `string` | UUIDv4 representing the entrant (`entrant_id`). See [Entrant](#raceentrant). | `12dbd4e3-0f0d-45f8-b1b2-1b2bf8747554`
`dividendCode` | `string` | Code representing the price. Derived from below list. | `fw`

List of available `dividendCodes`:

Code | Associated Product
--------- | -----------
`bob` | Best of the Best
`btsp` | Best Tote or Starting Price
`btp` | Best Tote Place
`fp` | Fixed Place
`fw` | Fixed Win
`mtsp` | Mid Tote or Starting Price
`mtp` | Mid Tote Place
`mtw` | Mid Tote Win
`spg` | Starting Price Guarantee
`tf` | Top Fluc
`tp` | Tote Place
`tw` | Tote Win

When constructed correctly, the URL used to access our platform should match a format similar to:

`https://www.ladbrokes.com.au/racing/bunbury/d3c7c5f8-5e4a-40e6-a1e7-69d6d9fd722d/12dbd4e3-0f0d-45f8-b1b2-1b2bf8747554/fw`

Routes will land a user within Betslip, with their bet added, and also route them to the associated racecard for the provided `raceId` following happy path.
There are a variety of conditions that can invalidate a route, which we handle differently depending on condition:

Error Case | Description | Result
--------- | ----------- | ---------
`Existing Errored Bets` | User contains errored bets in Betslip. | User routed to `Home`, no bet added, user notified via toast.
`Existing Processing Bets` | User contains processing bets in Betslip. | User routed to `Home`, no bet added, user notified via toast.
`Network Error` | Unable to fetch data required. | User routed to `Home`, no bet added, user notified via toast.
`Race Not Found` | Race doesn't exist on platform. | User routed to `Home`, no bet added, user notified via toast.
`Meeting Not Found` | Meeting doesn't exist on platform. | User routed to `Home`, no bet added, user notified via toast.
`Price Unavailable` | Price is no longer available on race. | User routed to `Race`, no bet added, user notified via toast.
`Market Unavailable` | Market is no longer available on race. | User routed to `Race`, no bet added, user notified via toast.
`Race Unavailable` | Race is no longer available. | User routed to `Race`, no bet added, user notified via toast.
`Entrant Unavailable` | Entrant is no longer available on race. | User routed to `Race`, no bet added, user notified via toast.
`Market Controlled` | Market is controlled due to Punter Assist condition. | User routed to `Race`, no bet added, user notified via toast, Market Control condition displayed.
