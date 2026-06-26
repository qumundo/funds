# Funds & ETFs Data
Fund and ETF data service for identifying, accessing and using fund and ETF information, analytics and insights including reference data, holdings data and primary market data. An ETF, or Exchange-Traded Fund, is a type of investment fund that bundles together and holds a basket of assets - like stocks, bonds, or commodities - and trades on a stock exchange. When you buy a share of an ETF, you are buying a portion of a larger pool of assets. The released services and APIs include a comprehensive set of tools and information like reference data, holdings data, pricing and primary market data, ranging from identifiers, classifications and meta data, constituent- and security-level weightings, asset class categorization and pricing information, as well net asset value (NAV), asset under management (AUM) and fund flow information.

Documentation, Examples, FAQ, Terms & Conditions and License: [https://www.qumundo.com/docs/funds-and-etfs-data](https://www.qumundo.com/docs/funds-and-etfs-data).

If you see a feature that is missing or not correctly enforced, please [open an issue](https://github.com/qumundo/funds/issues).

_You can find example filter parameter (key) and response schema in the JSON file here: [example.json](https://github.com/qumundo/funds/blob/main/res/example.json)_

**:de: Crafted in Frankfurt am Main, Germany.**

## Last changes

Last changes and the version history can be found in the [CHANGELOG.md](https://github.com/qumundo/funds/blob/master/CHANGELOG.md) file.

_As this library is SemVer-compatible, any breaking change would be released as a MAJOR version only. Non-breaking changes and features are released as MINOR. Feature updates and bug fixes are released as PATCH (note that feature updates may as well be bundled under a MINOR release, if it comes with new settings or minor changes)._

> **Beta Notice:**  
> This project is currently in the 0.x beta phase.  
> Under SemVer, versions below 1.0.0 may introduce breaking changes at any time.
> Coverage is still expanding. Issuers and data fields are being added continuously. If you have specific data requirements, let us know so we can prioritize them.

## How to install?

Include `funds` in your `package.json` dependencies:

```bash
npm install --save @qumundo/funds
```

## How to use?

This module may be used to identify, access and use fund and ETF information, analytics and insights including reference, holdings and primary market data. You may use multi-tag filter parameters (keys) to build customized queries to your needs.

### :link: Import the module

Import the module in your code:

`const Funds = require("@qumundo/funds")`

### :arrow_right: Get fund level fund and etf data

```javascript

let data = await Funds.getData({ path: "funds/search?" });

/*
data.data[0] ===
{
  "id": 368155,
  "date": "20260624",
  "name": "Amundi Prime Eurozone UCITS ETF DR (C)",
  "ticker": "",
  "isin": "LU2089238112",
  "wkn": "A2PWMJ",
  "sedol": "",
  "type": "ETF",
  "currency": "EUR",
  "shares": "2606485",
  "total_assets": "846960740.35",
  "nav": "38.39",
  "sfdr": "6",
  "ter": "0.0500",
  "inception": "20200115",
  "dated": "20260623"
}
*/
```

### :arrow_right: Get constituent level fund and etf data

```javascript

let data = await Funds.getData({ path: "funds/holdings/search?" });

/*
data.data[0] ===
{
  "id": 213283,
  "date": "20260624",
  "fund_ticker": "",
  "fund_isin": "LU2089238112",
  "weighting": "0.0982517",
  "name": "ASML HOLDING NV",
  "ticker": "ASML NA",
  "isin": "NL0010273215",
  "wkn": "",
  "sedol": "",
  "country": "Netherlands",
  "currency": "EUR",
  "market_price": "",
  "shares": "50964",
  "market_value": "",
  "dated": "20260622"
}
*/
```

### :arrow_right: Get overview of all databases (paths) available

```javascript

let example = await Funds.getExample();

/*
Object.keys(example) ===
[
  'funds/search?',
  'funds/holdings/search?'
]
*/
```

## Filtering

Filtering and customized requests can be built and accomplished by passing filter parameters (objects) to the .getData() instance:

```javascript
let items = [
  { key: 'isin', value: 'LU2089238112' }
];

let data = await Funds.getData({ path: "funds/search?", items });
```

The default filter syntax is LIKE: `=`.

Filtering for an exact value (MATCH) attach and change the syntax (filter):

```javascript
let items = [
  { key: 'name', value: 'Xtrackers World Equity Enhanced Active UCITS ETF 1C', filter: '==' }
];
```

If the search result should not contain a certain value, use a single equal and an exclamation =! sign:

```javascript
let items = [
  { key: 'name', value: 'Amundi', filter: '=!' }
];
```

When filtering for multiple values of a parameter, use comma separation:

```javascript
let items = [
  { key: 'name', value: 'Amundi,Xtrackers' }
];
```

To change the operator (default is 'OR') when searching for multiple values of a parameter, attach and change the parameter 'operator' of an object:

```javascript
let items = [
  { key: 'name', value: 'Amundi,Europe', operator: 'and' }
];
```

When filtering for multiple parameter add another object key:

```javascript
let items = [
  { key: 'name', value: 'Amundi' },
  { key: 'sfdr', value: '8' }
];
```

To change the operator (default is 'AND') when searching for multiple parameters, attach and change parameter operator in the .getData() instance:

```javascript
let items = [
  { key: 'name', value: 'Amundi' },
  { key: 'sfdr', value: '8' }
];

let data = await Funds.getData({ path: "funds/search?", items, operator: 'or' });
```

If the search result should be in between a range of values, use double dots '..' between two values:

```javascript
let items = [
  { key: 'nav', value: '100..200' }
];
```

Documentation and examples of available filter options can be found here: [https://www.qumundo.com/docs/introduction#filtering](https://www.qumundo.com/docs/introduction#filtering).

## Pagination

In many occasions too much information is returned in a single request. By default a maximum of 100 records per request will be returned. To receive more records per request, you may specify a `pageSize` (max. 250) parameter object. The response can then be paginated adding and using the `pageNum` parameter:

```javascript
let items = [
  { key: 'pageSize', value: '50' },
  { key: 'pageNum', value: '2' }
];

let data = await Funds.getData({ path: "funds/search?", items });
```

## Filter Keys and Example Responses

Get an overview of all filter parameter (key) for the fund level data available call the .getExample() instance with the path:

```javascript

let example = await Funds.getExample({ path: "funds/search?" });

/*
[
  { "key": "date", "field": "date", "type": "date" },
  { "key": "name", "field": "name", "type": "string" },
  { "key": "ticker", "field": "ticker", "type": "string" },
  { "key": "isin", "field": "isin", "type": "string" },
  { "key": "wkn", "field": "wkn", "type": "string" },
  { "key": "sedol", "field": "sedol", "type": "string" },
  { "key": "type", "field": "type", "type": "string" },
  { "key": "currency", "field": "currency", "type": "string" },
  { "key": "shares", "field": "shares", "type": "number" },
  { "key": "totalAssets", "field": "total_assets", "type": "number" },
  { "key": "nav", "field": "nav", "type": "number" },
  { "key": "sfdr", "field": "sfdr", "type": "number" },
  { "key": "ter", "field": "ter", "type": "number" },
  { "key": "inception", "field": "inception", "type": "date" },
  { "key": "dated", "field": "dated", "type": "date" }
]
*/
```

Get an overview of all filter parameter (key) for the holdings level data available call the .getExample() instance with the path:

```javascript

let example = await Funds.getExample({ path: "funds/holdings/search?" });

/*
[
  { "key": "date", "field": "date", "type": "date" },
  { "key": "fundTicker", "field": "fund_ticker", "type": "string" },
  { "key": "fundIsin", "field": "fund_isin", "type": "string" },
  { "key": "weighting", "field": "weighting", "type": "number" },
  { "key": "name", "field": "name", "type": "string" },
  { "key": "ticker", "field": "ticker", "type": "string" },
  { "key": "isin", "field": "isin", "type": "string" },
  { "key": "wkn", "field": "wkn", "type": "string" },
  { "key": "sedol", "field": "sedol", "type": "string" },
  { "key": "country", "field": "country", "type": "string" },
  { "key": "currency", "field": "currency", "type": "string" },
  { "key": "marketPrice", "field": "market_price", "type": "number" },
  { "key": "shares", "field": "shares", "type": "number" },
  { "key": "market_value", "field": "market_value", "type": "number" },
  { "key": "dated", "field": "dated", "type": "date" }
]
*/
```

Get an example response for a specific database (path) available:

```javascript

let example = await Funds.getExample(false, { path: "funds/holdings/search?" });

/*
"200": {
  "status": 200,
  "success": true,
  "record_count": 100,
  "total_records": 2770422,
  "page_number": 1,
  "page_size": 100,
  "total_pages": 27705,
  "more_pages": true,
  "data": [
    {
      "id": 213283,
      "date": "20260624",
      "fund_ticker": "",
      "fund_isin": "LU2089238112",
      "weighting": "0.0982517",
      "name": "ASML HOLDING NV",
      "ticker": "ASML NA",
      "isin": "NL0010273215",
      "wkn": "",
      "sedol": "",
      "country": "Netherlands",
      "currency": "EUR",
      "market_price": "",
      "shares": "50964",
      "market_value": "",
      "dated": "20260622"
    },
    {
      "id": 213284,
      "date": "20260624",
      "fund_ticker": "",
      "fund_isin": "LU2089238112",
      "weighting": "0.03128721",
      "name": "SIEMENS AG-REG",
      "ticker": "SIE GY",
      "isin": "DE0007236101",
      "wkn": "",
      "sedol": "",
      "country": "Germany",
      "currency": "EUR",
      "market_price": "",
      "shares": "96315",
      "market_value": "",
      "dated": "20260622"
    },
    {
      "id": 213285,
      "date": "20260624",
      "fund_ticker": "",
      "fund_isin": "LU2089238112",
      "weighting": "0.02712795",
      "name": "BANCO SANTANDER SA MADRID",
      "ticker": "SAN SM",
      "isin": "ES0113900J37",
      "wkn": "",
      "sedol": "",
      "country": "Spain",
      "currency": "EUR",
      "market_price": "",
      "shares": "1941956",
      "market_value": "",
      "dated": "20260622"
    },
    {
      "id": 213286,
      "date": "20260624",
      "fund_ticker": "",
      "fund_isin": "LU2089238112",
      "weighting": "0.02475573",
      "name": "SCHNEIDER ELECT SE",
      "ticker": "SU FP",
      "isin": "FR0000121972",
      "wkn": "",
      "sedol": "",
      "country": "France",
      "currency": "EUR",
      "market_price": "",
      "shares": "72629",
      "market_value": "",
      "dated": "20260622"
    },
    {
      "id": 213287,
      "date": "20260624",
      "fund_ticker": "",
      "fund_isin": "LU2089238112",
      "weighting": "0.02415756",
      "name": "ALLIANZ SE-REG",
      "ticker": "ALV GY",
      "isin": "DE0008404005",
      "wkn": "",
      "sedol": "",
      "country": "Germany",
      "currency": "EUR",
      "market_price": "",
      "shares": "51029",
      "market_value": "",
      "dated": "20260622"
    },
    {}
  ]
}
*/
```

## Need more fund and etf data?

Choose between various subscription options for your use case: [https://www.qumundo.com/api/funds-and-etfs-data](https://www.qumundo.com/api/funds-and-etfs-data).

Once subscribed and enabled, login to our platform and provide authentication along your request.

### :arrow_right: Login to our platform to receive an authentication token

```javascript
let credentials = { email: config.YOUR_EMAIL, password: config.YOUR_PASSWORD };   // Keep your login credentials secure and secret

let login = await Funds.getLogin(credentials);

let token = login.token;
```

### :arrow_right: Get data with your subscription

```javascript
let data = await Funds.getData({ path: "funds/search?" }, token);
```

_If you need additional features, changes or notice any discrepancies, feel free to submit a Pull Request._

**Consider supporting the development by [making a donation](https://github.com/sponsors/qumundo).**

## Need more data?

If you are missing any data in the current solution or in general, submit a Pull Request, contact us directly or register a Qumundo Account [https://www.qumundo.com/register/](https://www.qumundo.com/register/) and define your requirements.
