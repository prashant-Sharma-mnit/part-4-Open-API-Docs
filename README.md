# 📘 5paisa Xtream Open APIs Documentation Part4

---

## Table of Contents

### 8. Pre order Margin Calculation & ScripMaster
- [MultiOrderMargin](#multiordermargin) 
- [ScripMaster](#scripmaster) 
  
  
### 9. WebSocket 
- [WebSocket](#websocket)   
  
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
| `x-clientcode`              | Logged-in user's client ID        |

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

## 📦 Tags

`#MultiOrderMargin` `#5paisaAPI` `#RiskEngine` `#BasketTrading` `#MarginEstimation` `#AlgoTrading` `#OptionsMargin` `#Hedging`

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

## 🧪 Sample cURL Requests

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

# WebSocket 
---

## 🌐 Overview of WebSocket

Whether it's fetching live market data or receiving instant trade confirmations, real-time streaming support greatly enhances the experience for traders and investors.

The 5paisa Xstream WebSocket API allows seamless integration of real-time data feeds into trading applications with minimal latency.

### 🔑 Key Features

* **📈 Live Streaming of Market Data**: Access real-time price movements, quotes, and market depth.
* **🔔 Order & Trade Confirmations**: Get immediate updates on all order lifecycle events (Place, Modify, Cancel, Trigger).
* **🔐 Secure & Authenticated Access**: Requires valid `access_token` and `client_code`.

---

## ⚙️ How It Works

### 🔗 Step 1: Connect to WebSocket Server

Use the following URL with query parameters:

```
wss://openfeed.5paisa.com/feeds/api/chat?Value1=<access_token>|<client_code>
```

* Replace `<access_token>` and `<client_code>` with your credentials (from the access token API).

### 🔔 Step 2: Subscribe to Data Streams

Once connected, the client sends subscription requests to receive specific types of data (market feed, depth, trade confirmations, etc.).

### 🔁 Step 3: Manage Subscriptions

Clients can send `Subscribe` or `Unsubscribe` operations dynamically throughout the session.

---

## 💓 Heartbeat Mechanism

* When the **client sends** `PING`, the **server responds** with `PONG`.
* Ensures the connection remains alive.

---

## 🔁 WebSocket Methods

All messages are JSON objects with this structure:

```json
{
  "Method": "<method_name>",
  "Operation": "Subscribe | Unsubscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {
      "Exch": "N",
      "ExchType": "C",
      "ScripCode": 1660
    }
  ]
}
```

---
##  📋 Supported Methods Overview

|  Method                    | Purpose                                      |
| ------------------------- | -------------------------------------------- |
|  `MarketFeedV3`            | Real-time LTP, OHLC, bid/offer details       |
|  `MarketFeedLite`          | Minimal quote data (LTP, % change, last qty) |
| `MarketDepthService`      | Order book (bid/ask) depth                   |
| `GetScripInfoForFuture`   | Open interest & OI high/low for derivatives  |
|  `OrderTradeConfirmations` | Order lifecycle updates (placed, traded, SL) |

---


### 📈 Method: `MarketFeedV3`

* **Purpose**: Real-time quotes, LTP, OHLC, bid/ask

#### ✅ Subscribe Request

```json
{
  "Method": "MarketFeedV3",
  "Operation": "Subscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "C", "ScripCode": 1660}
  ]
}
```

#### 🔄 Unsubscribe Request

```json
{
  "Method": "MarketFeedV3",
  "Operation": "Unsubscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "C", "ScripCode": 1660}
  ]
}
```

#### 📤 Response Payload

```json
{
  "Exch": "N",
  "ExchType": "C",
  "Token": 1660,
  "LastRate": 440.1,
  "TotalQty": 1000,
  "High": 445.5,
  "Low": 435.1,
  "OpenRate": 438,
  "PClose": 440,
  "AvgRate": 441.2,
  "Time": 35950,
  "TickDt": "/Date(1718052145000)/"
}
```

---


### 🔢 Method: `MarketFeedLite`

* **Purpose**: Lightweight version of MarketFeedV3 for minimal bandwidth use

#### ✅ Subscribe Request

```json
{
  "Method": "MarketFeedLite",
  "Operation": "Subscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "C", "ScripCode": 1333},
    {"Exch": "N", "ExchType": "D", "ScripCode": 148108},
    {"Exch": "B", "ExchType": "C", "ScripCode": 500112}
  ]
}
```

#### 🔄 Unsubscribe Request

```json
{
  "Method": "MarketFeedLite",
  "Operation": "Unsubscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "C", "ScripCode": 1333}
  ]
}
```

#### 📤 Response Payload

```json
{
  "Exch": "N",
  "ExchType": "C",
  "Token": 1333,
  "LastRate": 523.8,
  "LastQty": 10,
  "TickDt": "/Date(1718052145000)/",
  "ChgPcnt": 0.45
}
```

### 📊 Method: `MarketDepthService`

* **Purpose**: Order book depth (bid/ask levels)

#### ✅ Subscribe Request

```json
{
  "Method": "MarketDepthService",
  "Operation": "Subscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "C", "ScripCode": 2885}
  ]
}
```

#### 🔄 Unsubscribe Request

```json
{
  "Method": "MarketDepthService",
  "Operation": "Unsubscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "C", "ScripCode": 2885}
  ]
}
```

#### 📤 Response Payload

```json
{
  "Exch": "N",
  "ExchType": "C",
  "Token": 2885,
  "TBidQ": 75000,
  "TOffQ": 82000,
  "Details": [
    {"Quantity": 500, "Price": 350.5, "NumberOfOrders": 3, "BbBuySellFlag": 66},
    {"Quantity": 450, "Price": 350.6, "NumberOfOrders": 2, "BbBuySellFlag": 83}
  ],
  "Time": "/Date(1718052145000)/"
}
```

---

### 📉 Method: `GetScripInfoForFuture`

* **Purpose**: Real-time Open Interest (OI) info

#### ✅ Subscribe Request

```json
{
  "Method": "GetScripInfoForFuture",
  "Operation": "Subscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "D", "ScripCode": 48508}
  ]
}
```

#### 🔄 Unsubscribe Request

```json
{
  "Method": "GetScripInfoForFuture",
  "Operation": "Unsubscribe",
  "ClientCode": "<client_code>",
  "MarketFeedData": [
    {"Exch": "N", "ExchType": "D", "ScripCode": 48508}
  ]
}
```

#### 📤 Response Payload

```json
{
  "Exch": "N",
  "ExchType": "D",
  "Token": 48508,
  "OpenInterest": 2388700,
  "DayHiOI": 2400000,
  "DayLoOI": 2345100
}
```

---

### 🔔 Method: `OrderTradeConfirmations`

* **Purpose**: Lifecycle status of orders (place/modify/cancel/trade/SL)

#### ✅ Subscribe Request

```json
{
  "Method": "OrderTradeConfirmations",
  "Operation": "Subscribe",
  "ClientCode": "<client_code>"
}
```

#### 🔄 Unsubscribe Request

```json
{
  "Method": "OrderTradeConfirmations",
  "Operation": "Unsubscribe",
  "ClientCode": "<client_code>"
}
```

#### 📤 Response Payload (Place Order Example)

```json
{
  "ReqType": "P",
  "Status": "Placed",
  "ScripCode": 1660,
  "Price": 425,
  "Qty": 1,
  "BuySell": "B",
  "ReqStatus": 0
}
```

#### 📤 Response Payload (Trade Example)

```json
{
  "ReqType": "T",
  "Status": "Fully Executed",
  "TradedQty": 1,
  "Price": 434.2,
  "ExchTradeTime": "2024-05-09 11:32:34"
}
```

---


#### ✅ Testing Considerations

* Validate correct fields only (minimal subset)
* Compare consistency with `MarketFeedV3`
* Validate against large inputs (up to 3000 scrips)
* Test concurrent subscriptions for stability

---

## 📘 Parameter Summary

### 🔸 Exch (Exchange)

| Code | Description |
| ---- | ----------- |
| N    | NSE         |
| B    | BSE         |
| M    | MCX         |

### 🔸 ExchType (Segment)

| Code | Description          |
| ---- | -------------------- |
| C    | Cash                 |
| D    | Derivatives (F\&O)   |
| U    | Currency Derivatives |


### 🔸 Operation

| Value       | Description               |
| ----------- | ------------------------- |
| Subscribe   | Start receiving live data |
| Unsubscribe | Stop receiving live data  |

---

## 📘 Best Practices

* Subscribe only to required scrips to manage bandwidth and rate limits.
* Use heartbeat (`PING`/`PONG`) to maintain connection health.
* Monitor server responses for real-time margin alerts or trade confirmations.
* Avoid exceeding **3000 subscriptions** per session.

---
---
