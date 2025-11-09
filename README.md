# 📘 5paisa Xtream Open APIs Documentation Part4

---

## Table of Contents

### 7. historical data 
- [HistoricalCandle](#historicalcandle)   

### 8. Pre order Margin Calculation & ScripMaster
- [MultiOrderMargin](#multiordermargin) 
- [ScripMaster](#scripmaster) 
  
 
---


# HistoricalCandle

---

## 🔹 API Name
`GET /V2/historical/{Exch}/{ExchType}/{ScripCode}/{Interval}`

---

## 🎯 Purpose

The **Historical Candles API** provides **OHLCV (Open, High, Low, Close, Volume)** data for a specified instrument over a given time range and interval granularity. It is essential for:

- 🧠 Backtesting trading strategies  
- 📊 Rendering candlestick charts  
- 📈 Deploying signal-generating indicators  
- 🤖 Feeding RAG-enabled trading assistants  
- 🔍 Analyzing historical price-action behavior  

---

## 🔗 Endpoint Template

```url
https://openapi.5paisa.com/V2/historical/{Exch}/{ExchType}/{ScripCode}/{Interval}?from={FromDate}&end={EndDate}
````

**Example:**

```
https://openapi.5paisa.com/V2/historical/N/C/1660/1d?from=2023-01-01&end=2023-03-01
```

---

## 📥 Request Overview

### ➕ HTTP Method

`GET`

### ➕ Required Headers

| Header Name                 | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| `Authorization`             | Bearer token issued after successful login (JWT format) |
| `x-clientcode`              | Unique identifier of the logged-in user                 |


---

## 🔣 URL Path Parameters

| Parameter   | Type   | Required | Description                                         |
| ----------- | ------ | -------- | --------------------------------------------------- |
| `Exch`      | string | ✅ Yes    | Exchange code (e.g., `N`, `B`, `M`, `X`)            |
| `ExchType`  | string | ✅ Yes    | Segment type (e.g., `C` for cash, `D` for F\&O)     |
| `ScripCode` | int    | ✅ Yes    | Unique instrument identifier (used to place orders) |
| `Interval`  | string | ✅ Yes    | Candle interval (`1m`, `5m`, `1d`, etc.)            |

---

## 📅 Query Parameters

| Parameter | Type | Required | Description                       |
| --------- | ---- | -------- | --------------------------------- |
| `from`    | date | ✅ Yes    | Start date (format: `YYYY-MM-DD`) |
| `end`     | date | ✅ Yes    | End date (format: `YYYY-MM-DD`)   |

---

## 🧪 Sample cURL Request

```bash
curl --location 'https://openapi.5paisa.com/V2/historical/N/C/1660/1d?from=2023-01-01&end=2023-01-10' \
--header 'Authorization: Bearer <YourAccessToken>' \
--header 'x-clientcode: <YourClientCode>' \
```

---

## 📤 Success Response Format

```json
{
  "status": "success",
  "data": {
    "candles": [
      [
        "2023-01-01T00:00:00",
        292.0,
        294.0,
        289.5,
        293.55,
        11025420
      ],
      [
        "2023-01-02T00:00:00",
        295.0,
        296.3,
        293.75,
        295.3,
        11315876
      ]
    ]
  }
}
```

Each array in `candles` represents a single candle for the given interval.

---

## ❌ Failure Response Format

```json
{
  "head": {
    "ResponseCode": "RPOpenAPI",
    "Status": 1,
    "Status_description": "Error While Processing"
  },
  "body": null
}
```

---

## 📘 Field Breakdown

### 🔹 Candle Array Structure

Each element in the `candles` list is an ordered array:

| Index | Field     | Type     | Description                                      |
| ----- | --------- | -------- | ------------------------------------------------ |
| \[0]  | Timestamp | datetime | Start time of the candle (`YYYY-MM-DDTHH:MM:SS`) |
| \[1]  | Open      | float    | Opening price for the candle period              |
| \[2]  | High      | float    | Highest price during the period                  |
| \[3]  | Low       | float    | Lowest price during the period                   |
| \[4]  | Close     | float    | Closing price for the candle period              |
| \[5]  | Volume    | integer  | Total traded volume during the period            |

---

## ⏱ Supported Intervals

| Code | Description | Typical Use Case                |
| ---- | ----------- | ------------------------------- |
| 1m   | 1 Minute    | Scalping, short-term signals    |
| 5m   | 5 Minutes   | Intraday pattern detection      |
| 10m  | 10 Minutes  | Low-frequency intraday signals  |
| 15m  | 15 Minutes  | Swing entries                   |
| 30m  | 30 Minutes  | Half-hour OHLC confirmation     |
| 60m  | 1 Hour      | Hourly trend observation        |
| 1d   | Daily       | Backtesting, EOD strategy logic |

---

## 🏦 Exchange Codes

| Code | Exchange          |
| ---- | ----------------- |
| N    | NSE               |
| B    | BSE               |
| M    | MCX (Commodities) |
| X    | NCDEX             |

---

## 🧾 Exchange Segment Codes

| Code | Segment Description               |
| ---- | --------------------------------- |
| C    | Cash Segment (Stocks)             |
| D    | Derivatives (Futures and Options) |
| U    | Currency Derivatives              |
| Y    | NSE & BSE Commodities             |
| X    | NCDEX Commodities                 |

---

## 📎 Notes & Limitations

* ✅ **Max Interval Duration**:

  * 1-minute candles: up to 6 months max
  * 1-day candles: entire historical range allowed

* ⏳ Timestamps are in **ISO 8601 format**: `YYYY-MM-DDTHH:MM:SS`

* ⚠️ Always validate JWT tokens via the dedicated token validation API before usage

* 🔁 Frequent refresh of recent candles (especially in live trading systems) is recommended

---

## 🧠 Ideal Use Cases

| Scenario                        | Why It's Ideal                             |
| ------------------------------- | ------------------------------------------ |
| Backtesting trading strategies  | Provides clean OHLCV for signal simulation |
| Chart rendering in trading apps | Supplies ready-to-plot candle arrays       |
| RAG-based AI trading assistants | Used as memory-grounded signal facts       |
| Automated signal generators     | Serves as the data source for triggers     |
| Momentum / Volume screening     | Used to compute technical indicators       |

---

## 📦 Tags

`#HistoricalCandles` `#OHLCV` `#AlgoTrading` `#Backtesting` `#5paisaAPI` `#MarketData` 

---


# MultiOrderMargin

---

## 🔹 API Name

`MultiOrderMargin`

---

## 📌 Endpoint

```http
POST https://openapi.5paisa.com/VendorsAPI/Service1.svc/MultiOrderMargin
```

---

## 🎯 Purpose

The `MultiOrderMargin` API enables you to **calculate margin requirements for multiple orders in a single request**. It simplifies and accelerates margin evaluation, especially for:

* 🔁 Portfolio rebalancing
* 🧠 Strategy-based trade execution
* 🚨 Fast market response mechanisms
* 🛒 Basket trading or multi-leg orders
* 🔄 Margin optimization based on existing holdings

This is crucial for **trading bots**, **automated allocators**, **risk managers**, and **high-frequency trading systems**.

---

## 💡 Key Advantages

* 📦 **Multi-order bundling**: No need to submit orders one by one.
* ⚡ **Faster decision-making**: Real-time feedback on margin requirements.
* 🧮 **Smart auto-offset**: Automatically accounts for existing positions (if `CoverPositions` = `"Y"`).
* 📈 **Strategy efficiency**: Perfect for algo rebalancing, hedging, and scalping.
* 🔐 **Avoids unexpected rejections**: Especially useful in F\&O with margin-intensive strategies.

---

## 🔍 Real-World Examples

### Scenario 1 — Cash Segment

A trader wants to rebalance their portfolio with:

* Buy 20 shares of Infosys (INFY)
* Sell 10 shares of Reliance (RELIANCE)
* Buy 15 shares of TCS

The API returns the total margin required for this multi-stock order.

### Scenario 2 — Options Strategy (F\&O)

A trader is planning a Nifty straddle:

* Sell 1 lot of Nifty 50 ATM Call Option
* Sell 1 lot of Nifty 50 ATM Put Option

The API returns the margin for both legs together, **factoring in margin benefit due to hedging** if `CoverPositions: "Y"`.

---

## 📥 Request Overview

### ➕ HTTP Method

`POST`

### ➕ Required Headers

| Header Name                 | Description                       |
| --------------------------- | --------------------------------- |
| `Authorization`             | Bearer token after login (JWT)    |
| `Content-Type`              | `application/json`                |

---

## 🔸 Request Body Structure

```json
{
  "head": {
    "key": "<YourAppKey>"
  },
  "body": {
    "ClientCode": "<YourClientCode>",
    "CoverPositions": "Y",
    "Orders": [
      {
        "Exch": "N",
        "ExchType": "C",
        "ScripCode": 11915,
        "ScripData": "",
        "PlaceModifyCancel": "P",
        "OrderType": "B",
        "Price": 0,
        "Qty": 1,
        "IsIntraday": false
      },
      {
        "Exch": "N",
        "ExchType": "C",
        "ScripCode": 438,
        "ScripData": "",
        "PlaceModifyCancel": "P",
        "OrderType": "S",
        "Price": 0,
        "Qty": 10,
        "IsIntraday": false
      }
    ]
  }
}
```

## 🧪 Sample cURL Request

```bash
curl --location 'https://openapi.5paisa.com/VendorsAPI/Service1.svc/MultiOrderMargin' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <YourAccessToken>' \
--data '{
  "head": {
    "key": "<YourAppKey>"
  },
  "body": {
    "ClientCode": "<YourClientCode>",
    "CoverPositions":"Y",
    "Orders": [
      {
        "Exch": "N",
        "ExchType": "C",
        "ScripCode": 11915,
        "PlaceModifyCancel": "P",
        "OrderType": "B",
        "Price": 0,
        "Qty": 20,
        "IsIntraday": false
      },
      {
        "Exch": "N",
        "ExchType": "C",
        "ScripCode": 438,
        "PlaceModifyCancel": "P",
        "OrderType": "S",
        "Price": 0,
        "Qty": 10,
        "IsIntraday": false
      },
      {
        "Exch": "N",
        "ExchType": "D",
        "ScripCode": 999920003,
        "PlaceModifyCancel": "P",
        "OrderType": "S",
        "Price": 0,
        "Qty": 75,
        "IsIntraday": true
      }
    ]
  }
}'
```

---

---

## 🧾 Field Descriptions

| Field               | Type    | Description                                               |
| ------------------- | ------- | --------------------------------------------------------- |
| `Exch`              | string  | Exchange code (`N`, `B`, `M`, `X`)                        |
| `ExchType`          | string  | Segment type (`C`=Cash, `D`=F\&O, `U`=Currency)           |
| `ScripCode`         | int     | Instrument identifier for the trade                       |
| `ScripData`         | string  | Optional tradable symbol name (if ScripCode is not given) |
| `PlaceModifyCancel` | string  | "P" = place, "M" = modify, "C" = cancel                   |
| `OrderType`         | string  | "B" = Buy, "S" = Sell                                     |
| `Price`             | float   | Limit price (set to 0 for market orders)                  |
| `Qty`               | int     | Quantity of shares/contracts                              |
| `IsIntraday`        | boolean | `true` for intraday, `false` for delivery                 |
| `CoverPositions`    | string  | "Y" to include existing holdings, "N" to ignore them      |

---



## ✅ Success Response

```json
{
  "body": {
    "AvailableMargin": 293657.29,
    "Message": "Success",
    "Status": 0,
    "TotalMarginRequired": 142.33
  },
  "head": {
    "responseCode": "5PMultiOrderMargin",
    "status": "0",
    "statusDescription": "Success"
  }
}
```

---

## ❌ Failure Response Examples

### ⚠️ Input Exceeds Order Limit

```json
{
  "body": {
    "Message": "One Time PreOrderMarginCalc Limit Exceed.",
    "Status": 1
  }
}
```

### ⚠️ Invalid Session

```json
{
  "body": {
    "Message": "Invalid Session",
    "Status": 9
  }
}
```

---

## 📘 Code References

### Exchange (`Exch`)

| Code | Exchange |
| ---- | -------- |
| N    | NSE      |
| B    | BSE      |
| M    | MCX      |
| X    | NCDEX    |

### Segment (`ExchType`)

| Code | Segment Description             |
| ---- | ------------------------------- |
| C    | Cash Segment (Equity)           |
| D    | Derivatives (Futures & Options) |
| U    | Currency Derivatives            |
| Y    | NSE/BSE Commodities             |

---

## 🧠 Ideal Use Cases

| Scenario                 | Why It Helps                                       |
| ------------------------ | -------------------------------------------------- |
| 📦 Basket Orders         | Submit multiple trades and compute combined margin |
| ⚖️ Portfolio Rebalancing | Auto-calculates required margin with offsets       |
| 📈 Strategy Execution    | Fits complex execution models (pairs, multi-leg)   |
| 🛡️ Options Hedging      | Accounts for margin benefit from net exposures     |

---

## 📎 Additional Notes

* Up to **50 orders** can be included in one request.
* `CoverPositions: "Y"` automatically reduces margin if you hold offsetting positions.
* Use `"N"` for raw margin preview **without** portfolio offset.
* Supports intraday and delivery-based order mix.
* Ensure session token is valid and active.
* Timestamps and calculations assume live market environment.

---

# ScripMaster

---

## 🔹 API Name
**ScripMaster**

---

## 🎯 Purpose

The `ScripMaster` API provides a downloadable, compressed **CSV dump** containing metadata for all instruments (equities, derivatives, currencies, commodities) across segments supported by the broker (e.g., NSE, BSE, MCX).

This data enables:

- ✅ Instrument discovery  
- ✅ Symbol-to-scripcode mapping for placing orders  
- ✅ Backtesting engine configuration  
- ✅ Trading assistant symbol resolution  
- ✅ Dropdown population in UIs  

---

## 📌 Endpoint

```

GET https://openapi.5paisa.com/VendorsAPI/Service1.svc/ScripMaster/segment/{segment}

````

---
## 🧾 Segment Options

The `{segment}` path parameter determines which scrip category to fetch:

| Segment     | Description                       |
|-------------|-----------------------------------|
| `all`       | All segments combined             |
| `nse_eq`    | NSE Equity                        |
| `bse_eq`    | BSE Equity                        |
| `nse_fo`    | NSE Futures & Options             |
| `bse_fo`    | BSE Futures & Options             |
| `ncd_fo`    | Currency Derivatives (NSE)        |
| `mcx_fo`    | MCX Commodities (Futures & Options)|

---

## 📥 Request Details

### ➕ Method
`GET`

---

## 🧪 ScripMaster cURL Requests

```bash
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/ScripMaster/segment/all' \
--header 'Content-Type: application/json'
````

```bash
# NSE Equity
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/ScripMaster/segment/nse_eq' \
--header 'Content-Type: application/json' 
````

```bash
# All segments combined
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/ScripMaster/segment/all' \
--header 'Content-Type: application/json' 
```

---





## ✅ Success Response

The API returns a **GZIP-compressed CSV file** with header:

```
Content-Type: application/octet-stream
Content-Encoding: gzip
Content-Disposition: attachment; filename=ScripMaster_{segment}.csv.gz
```

---

### 📄 Sample CSV Row (Decompressed)

```csv
Exch,ExchType,ScripCode,Name,Expiry,ScripType,StrikeRate,FullName,TickSize,LotSize,QtyLimit,Multiplier,SymbolRoot,BOCOAllowed,ISIN,ScripData,Series
N,C,1660,ITC,,EQ,0,ITC LTD,0.05,1,0,1,ITC,Y,INE154A01025,ITC_EQ,EQ
```

---

## ❌ Failure Response

### 1. Invalid segment

```text
Invalid segment 'xyz'. Allowed values are: all, bse_eq, nse_eq, nse_fo, bse_fo, ncd_fo, mcx_fo
```

### 2. Cache not available

```text
Cache not available for segment 'nse_fo'. Retry after some time.
```

---

## 📘 CSV Column Definitions

| Column        | Description                                                   |
| ------------- | ------------------------------------------------------------- |
| `Exch`        | Exchange code: `N`=NSE, `B`=BSE, `M`=MCX                      |
| `ExchType`    | Segment: `C`=Cash, `D`=Derivative, `U`=Currency               |
| `ScripCode`   | Unique numerical ID used to place orders                      |
| `Name`        | Contract or scrip name (may include expiry & strike for F\&O) |
| `Expiry`      | Expiry date (for F\&O), blank for equities                    |
| `ScripType`   | `CE`, `PE`, `EQ`, `XX`                                        |
| `StrikeRate`  | Strike price (for options)                                    |
| `FullName`    | Long name or description of the instrument                    |
| `TickSize`    | Minimum price movement (e.g., 0.05)                           |
| `LotSize`     | Lot size for contract                                         |
| `QtyLimit`    | Maximum quantity allowed per order                            |
| `Multiplier`  | Used to scale quantity (e.g., for currency pairs like USDINR) |
| `SymbolRoot`  | Underlying asset (e.g., NIFTY, RELIANCE)                      |
| `BOCOAllowed` | `Y` or `N` — Whether Bracket/Cover orders are permitted       |
| `ISIN`        | Security identifier (for equity stocks only)                  |
| `ScripData`   | Tradable symbol used in order placement                       |
| `Series`      | Typically `EQ` for equities, `XX` for derivatives             |

---

## 🔁 ScripData Format Guidelines

| Contract Type | Format Example                |
| ------------- | ----------------------------- |
| **Equity**    | `RELIANCE_EQ`                 |
| **Futures**   | `NIFTY_20240926`              |
| **Options**   | `BANKNIFTY_20241128_CE_43500` |

> Use `ScripData` and `ScripCode` to place orders with correct mapping.

---

## 🧠 Best Practices

* ⚠️ Always cache the ScripMaster locally and refresh once a day or during startup.
* ✅ Use `ScripCode` for placing orders; use `ScripData` to show users human-readable symbols.
* ⏱️ The CSV is updated regularly, usually once per trading day.
* 🔎 Use proper filters on `ExchType` and `ScripType` to identify relevant contracts.
* 🧪 Validate BO/CO eligibility using the `BOCOAllowed` column.

---

## 🔄 Use Cases

| Use Case                         | How it helps                                |
| -------------------------------- | ------------------------------------------- |
| Trading Bot Deployment           | Maps user inputs to valid `ScripCode`       |
| Strategy Backtesting Engine      | Loads instruments with lot size & tick size |
| Dashboard Drop-down Population   | Auto-populate symbol selectors              |
| Bracket/Cover Order Check        | Use `BOCOAllowed` to restrict UI elements   |
| Symbol Resolver in AI Assistants | Powers natural language mapping             |

---

## 📎 Additional Notes

* All responses are compressed in `.gz` format for bandwidth optimization.
* Format is CSV with UTF-8 encoding. Decompress and parse before using.
* Timestamps are not part of the data; treat this as a static master dump.


---
---
