# t(:websocket)
## t(:websocketauthentication)
> t(:websocket_codequote_auth_spot)

```python--pybit
# based on: https://github.com/bybit-exchange/pybit/blob/master/pybit/_http_manager.py

import hmac
import json
import time
import websocket

api_key = ""
api_secret = ""

# Generate expires.
expires = int((time.time() + 1) * 1000)

# Generate signature.
signature = str(hmac.new(
    bytes(api_secret, "utf-8"),
    bytes(f"GET/realtime{expires}", "utf-8"), digestmod="sha256"
).hexdigest())

ws = websocket.WebSocketApp(
    url=url,
    ...
)

# Authenticate with API.
ws.send(
    json.dumps({
        "op": "auth",
        "args": [api_key, expires, signature]
    })
)
```

<aside class="notice">
t(:spot_websocket_endpoints_aside)
</aside>

t(:spot_websocket_para_endpoint)

<aside class="notice">
t(:websocket_aside_auth)
</aside>


t(:websocket_para_methods)

t(:websocket_max_50_connection)

<aside class="notice">
t(:websocket_aside_signature)
</aside>

<aside class="warning">
t(:websocket_best_practices)
</aside>


## t(:heartbeat)
> t(:websocket_codequote_heartbeat)

```javascript
ws.send(JSON.stringify({"ping": 1535975085052}));
```

> t(:codequote_responseExample)

```javascript
{"pong": 1535975085152}
```


<aside class="warning">
t(:websocket_aside_heartbeat)
</aside>

<!-- 连接数限制
## t(:websocketlimit)
t(:websocket_para_limit)
-->


## t(:subscribe)
### t(:websocketfilters)

> t(:websocket_codequote_filters1)

```javascript
// Subscribing to the trade data for BTCUSDT
ws.send('{"symbol":"BTCUSDT","topic":"trade","event":"sub","params":{"binary":false}}');
```

> t(:spot_websocket_many_symbol_desc)

```javascript
// Example: Subscribing to the trade data for BTCUSDT and ETHUSDT
ws.send('{"symbol":"BTCUSDT,ETHUSDT","topic":"trade","event":"sub","params":{"binary":false}}');
```

t(:spot_websocket_subscribe_desc)

### t(:websocketunfilters)

t(:spot_websocket_unsubscribe_desc)

## t(:websocketresponse)
> t(:websocket_codequote_response)

```javascript
{
  "topic":"trade",
  "event":"sub",
  "symbol":"BTCUSDT",
  "params":{
    "binary":"false"
  },
  "code":"0",
  "msg":"Success"
}
```

t(:spot_websocket_para_response)

## t(:publictopics)
### t(:websockettrade)
> t(:codequote_subscribe)

```javascript
{
    "topic": "trade",
    "event": "sub",
    "symbol": "BTCUSDT",
    "params": {
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# To subscribe to multiple symbols,
# pass a list: ["BTCUSDT", "ETHUSDT"]
ws_spot.trade_v1_stream(
    handle_message, ["BTCUSDT", "ETHUSDT"]
)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
    "symbol":"BTCUSDT",
    "symbolName":"BTCUSDT",
    "topic":"trade",
    "params":{
        "realtimeInterval":"24h",
        "binary":"false"
    },
    "data":[
        {
            "v":"929681067596857345",
            "t":1625562619577,
            "p":"34924.15",
            "q":"0.00027",
            "m":true
        }
    ],
    "f": true,
    "sendTime": 1626249138535,
    "shared": false
}
```

t(:spot_websocket_trade_desc_v1)

t(:spot_public_websocket_frequency_300_delay_400)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| topic | string | t(:spot_topic) |
| binary | string | t(:spot_binary) |
| v | string | t(:spot_transactId) |
| t | string | t(:spot_time) |
| p | string | t(:spot_price)|
| q | boolean | t(:spot_quantity) |
| m | boolean | t(:spot_sMessage) |
| f | boolean | t(:spot_first) |
| sendTime | string | t(:spot_sendTime) |


### t(:websocketrealtimes)
> t(:codequote_subscribe)

```javascript
{
    "topic": "realtimes",
    "event": "sub",
    "symbol": "BTCUSDT",
    "params": {
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# To subscribe to multiple symbols,
# pass a list: ["BTCUSDT", "ETHUSDT"]
ws_spot.realtimes_v1_stream(
    handle_message, ["BTCUSDT", "ETHUSDT"]
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "symbol": "BTCUSDT",
  "symbolName": "BTCUSDT",
  "topic": "realtimes",
  "data": [{
    "t": "1565592599015",
    "s": "BTCUSDT",
    "sn": "BTCUSDT",
    "c": "12750.63",
    "h": "12800.96",
    "l": "12740.78",
    "o": "12780.23",
    "v": "73013.575",
    "qv": "15726612.498168",
    "m": "-0.0401",
    "e": 301
    }],
  "f": false,
  "sendTime": 1626252046034,
  "shared": false
}
```

t(:spot_websocket_ticker_desc_v1)

t(:spot_public_websocket_frequency_300_delay_400)

<p class="fake_header">t(:responseparameters)</p>
|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| topic | string | t(:spot_topic) |
| t | number | t(:spot_time) |
| s | string | t(:spot_symbol) |
| sn | string | t(:spot_symbol) |
| c | string | t(:spot_close)|
| h | string | t(:spot_high)|
| l | string | t(:spot_low)|
| o | string | t(:spot_open)|
| v | string | t(:spot_volume)|
| qv | string | t(:spot_quote_volume)|
| m | string | t(:spot_gains)|
| e | number | t(:spotExchangeId)|
| f | boolean | t(:spot_first) |
| sendTime | string | t(:spot_sendTime) |


### t(:websocketkline)
> t(:codequote_subscribe)

```javascript
{
    "topic": "kline_1m",
    "event": "sub",
    "symbol": "BTCUSDT",
    "params": {
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# To subscribe to multiple symbols,
# pass a list: ["BTCUSDT", "ETHUSDT"]
# pass an inverval
ws_spot.kline_v1_stream(
    handle_message, ["BTCUSDT", "ETHUSDT"], "1h"
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "symbol": "BTCUSDT",
  "symbolName": "BTCUSDT",
  "topic": "kline",
  "params": {
    "realtimeInterval": "24h",
    "klineType": "15m",
    "binary": "false"
  },
  "data": [{
    "t": 1565595900000,
    "s": "BTCUSDT",
    "sn": "BTCUSDT",
    "c": "11436.14",
    "h": "11437",
    "l": "11381.89",
    "o": "11381.89",
    "v": "16.3306"
  }],
  "f": true,
  "sendTime": 1626252389284,
  "shared": false
}
```

t(:spot_websocket_kline_desc_v1)

t(:spot_public_websocket_frequency_300_delay_400)

<p class="fake_header">t(:responseparameters)</p>
|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| topic | string | t(:spot_topic) |
| klineType | string | t(:spot_kline_type) |
| binary | string | t(:spot_binary) |
| t | number | t(:row_comment_resp_open_time) |
| s | string | t(:spot_symbol) |
| sn | string | t(:spot_symbol) |
| c | string | t(:spot_close)|
| h | string | t(:spot_high)|
| l | string | t(:spot_low)|
| o | string | t(:spot_open)|
| v | string | t(:spot_volume)|
| f | boolean | t(:spot_first) |
| sendTime | string | t(:spot_sendTime) |


### t(:websocketdepth)
> t(:codequote_subscribe)

```javascript
{
    "topic": "depth",
    "event": "sub",
    "symbol": "BTCUSDT",
    "params": {
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# To subscribe to multiple symbols,
# pass a list: ["BTCUSDT", "ETHUSDT"]
ws_spot.depth_v1_stream(
    handle_message, "ETHUSDT"
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "symbol": "BTCUSDT",
  "symbolName": "BTCUSDT",
  "topic": "depth",
  "params": {
    "realtimeInterval": "24h",
    "binary": "false"
  },
  "data": [{
    "e": 301,
    "s": "BTCUSDT",
    "t": 1565600357643,
    "v": "112801745_18",
    "b": [
      ["11371.49", "0.0014"],
      ["11371.12", "0.2"],
      ["11369.97", "0.3523"],
      ["11369.96", "0.5"],
      ["11369.95", "0.0934"],
      ["11369.94", "1.6809"],
      ["11369.6", "0.0047"],
      ["11369.17", "0.3"],
      ["11369.16", "0.2"],
      ["11369.04", "1.3203"],
    ]
    "a": [
      ["11375.41", "0.0053"],
      ["11375.42", "0.0043"],
      ["11375.48", "0.0052"],
      ["11375.58", "0.0541"],
      ["11375.7", "0.0386"],
      ["11375.71", "2"],
      ["11377", "2.0691"],
      ["11377.01", "0.0167"],
      ["11377.12", "1.5"],
      ["11377.61", "0.3"]
    ],
    "o": 0
  }],
  "f": true,
  "sendTime": 1626253839401,
  "shared": false
}
```

t(:spot_websocket_orderbook_desc_v1)

t(:spot_public_websocket_frequency_300_delay_650)

<p class="fake_header">t(:responseparameters)</p>
|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:----- |----- |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| topic | string | t(:spot_topic) |
| binary | string | t(:spot_binary) |
| e | number | t(:spotExchangeId) |
| t | number | t(:spot_timestamp2) |
| s | string | t(:spot_symbol) |
| v | string | t(:spot_version) |
| b | array | t(:spot_buys) |
| a | array | t(:spot_sells) |
| f | boolean | t(:spot_first) |
| o | number | t(:spotIgnore) |
| sendTime | string | t(:spot_sendTime) |

### t(:websocketmergeddepth)
> t(:codequote_subscribe)

```javascript
{
    "topic": "mergedDepth",
    "event": "sub",
    "symbol": "BTCUSDT",
    "params": {
        "binary": false,
        "dumpScale": 1
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# To subscribe to multiple symbols,
# pass a list: ["BTCUSDT", "ETHUSDT"]
# pass dumpScale
ws_spot.merged_depth_v1_stream(
    handle_message, "ETHUSDT", 1
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "symbol":"ETHUSDT",
  "symbolName":"ETHUSDT",
  "topic":"mergedDepth",
  "params":{"
    realtimeInterval":"24h",
    "dumpScale":"1",
    "binary":"false"
    },
  "data":[{
    "e":301,
    "s":"ETHUSDT",
    "t":1622541360174,
    "v":"10544_1",
    "b":[
          ["12001","1"],
          ["12000","0.8"],
          ["10000","1"],
          ["4988","0.5"],
          ["4987","0.8"],
          ["4986","1"],
          ["4985","0.9"],
          ["4984","1.1"],
          ["4983","1.3"],
          ["4982","0.5"],
          ["100000","0.94"]
        ],
    "a": [
          ["12001","1"],
          ["12000","0.8"],
          ["10000","1"],
          ["4988","0.5"],
          ["4987","0.8"],
          ["4986","1"],
          ["4985","0.9"],
          ["4984","1.1"],
          ["4983","1.3"],
          ["4982","0.5"],
          ["100000","0.94"]
    ],
    "o":0
    }],
    "f":false,
    "sendTime":1622541360301,
    "shared":false
}
```

t(:spot_websocket_orderbook_merge_desc_v1)

t(:spot_public_websocket_frequency_300_delay_650)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:----- |----- |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| topic | string | t(:spot_topic) |
| dumpScale | string | t(:spot_dumpScale) |
| binary | string | t(:spot_binary) |
| e | number | t(:spotExchangeId) |
| t | number | t(:spot_timestamp2) |
| s | string | t(:spot_symbol) |
| v | string | t(:spot_version) |
| b | array | t(:spot_buys) |
| a | array | t(:spot_sells) |
| f | boolean | t(:spot_first) |
| sendTime | string | t(:spot_sendTime) |

### t(:websocketdiffdepth)
> t(:codequote_subscribe)

```javascript
{
    "topic": "diffDepth",
    "event": "sub",
    "symbol": "BTCUSDT",
    "params": {
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# To subscribe to multiple symbols,
# pass a list: ["BTCUSDT", "ETHUSDT"]
ws_spot.diff_depth_v1_stream(
    handle_message, "ETHUSDT"
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "symbol": "BTCUSDT",
  "symbolName": "BTCUSDT",
  "topic": "diffDepth",
  "params": {
    "realtimeInterval": "24h",
    "binary": "false"
  },
  "data": [{
    "e": 301,
    "s": "BTCUSDT",
    "t": 1565600357643,
    "v": "112801745_18",
    "b": [
      ["11371.49", "0.0014"],
      ["11371.12", "0.2"],
      ["11369.97", "0.3523"],
      ["11369.96", "0.5"],
      ["11369.95", "0.0934"],
      ["11369.94", "1.6809"],
      ["11369.6", "0.0047"],
      ["11369.17", "0.3"],
      ["11369.16", "0.2"],
      ["11369.04", "1.3203"],
    "a": [
      ["11375.41", "0.0053"],
      ["11375.42", "0.0043"],
      ["11375.48", "0.0052"],
      ["11375.58", "0.0541"],
      ["11375.7", "0.0386"],
      ["11375.71", "2"],
      ["11377", "2.0691"],
      ["11377.01", "0.0167"],
      ["11377.12", "1.5"],
      ["11377.61", "0.3"]
    ],
    "o": 0
  }],
  "f": true,
  "sendTime": 1626253839401,
  "shared": false
}
```

t(:spot_websocket_orderbook_delta_desc_v1)

t(:spot_public_websocket_frequency_300_delay_650)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| topic | string | t(:spot_topic) |
| binary | string | t(:spot_binary) |
| e | number | t(:spotExchangeId) |
| t | number | t(:spot_timestamp2) |
| s | string | t(:spot_symbol) |
| v | string | t(:spot_version) |
| b | array | t(:spot_buys) |
| a | array | t(:spot_sells) |
| f | boolean | t(:spot_first) |
| o | number | t(:spotIgnore) |
| sendTime | string | t(:spot_sendTime) |


### t(:websocketLtNetvalue)
> t(:codequote_subscribe)

```javascript
{
    "topic": "lt",
    "symbol": "BTC3LUSDTNAV",
    "event": "sub",
    "params": {
        "binary": false
    }
}
```

```python--pybit

```

> t(:codequote_responseExampleFormatAll)

```javascript
{
    "symbol":"BTC3LUSDTNAV",
    "symbolName":"BTC3LUSDTNAV",
    "topic":"lt",
    "params":{
        "realtimeInterval":"24h",
        "binary":"false"
    },
    "data":[
        {
            "t":1650272320201,
            "s":"BTC3LUSDTNAV",
            "nav":"8.36157024279442973500",
            "b":"0.000741332244224795",
            "l":"3.448586744632192167",
            "loan":"-20.474030060817421380",
            "ti":"18969.901970158967556311",
            "n":"14.063000000262307568"
        }
    ],
    "f":"True",
    "sendTime":1650272320374
}
```
t(:spot_websocket_lt_nav_desc_v1)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| topic | string | t(:spot_topic) |
| binary | string | t(:spot_binary) |
| t | number | t(:spot_timestamp) |
| s | string | t(:spotLtSymbolNav) |
| nav | string | t(:spotResNetValue) |
| b | string | t(:spotLtQuoteBasket) |
| l | string | t(:spotLeverage) |
| loan | string | t(:spotLoan) |
| ti | string | t(:spotResCirculation) |
| n | string | t(:spotTotalPositionValue) |
| sendTime | string | t(:spot_sendTime) |


## t(:publictopics_v2)
### t(:websocketv2depth)
> t(:codequote_subscribe)

```javascript
{
    "topic": "depth",
    "event": "sub",
    "params": {
        "symbol": "BTCUSDT",
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# v2 does not support to subscribe mutiple symbols
ws_spot.depth_v2_stream(
    handle_message, "ETHUSDT"
)
while True:
    sleep(1)
```

> t(:codequote_snapshot)

```javascript
{
  "topic": "depth",
  "params": {
    "symbol": "BTCUSDT",
    "binary": "false",
    "symbolName": "BTCUSDT"
  },
  "data": {
    "s": "BTCUSDT",
    "t": 1582001376853,
    "v": "13850022_2",
    "b": [
      [
        "9780.79",
        "0.01"
      ],
      [
        "9780.5",
        "0.1"
      ],
      [
        "9780.4",
        "0.517813"
      ], ...
    "a": [
      [
        "9781.21",
        "0.042842"
      ],
      [
        "9782",
        "0.3"
      ],
      [
        "9782.1",
        "0.226"
      ], ...
    ]
  }
}
```
t(:spot_websocket_orderbook_desc_v2)

t(:spot_public_websocket_frequency_100)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| topic | string | t(:spot_topic) |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| symbolName | string | t(:spot_symbol) |
| binary | string | t(:spot_binary) |
| t | number | t(:spot_timestamp2) |
| s | string | t(:spot_symbol) |
| v | string | t(:spot_version) |
| b | string | t(:spot_buy) |
| a | string | t(:spot_sell) |

### t(:websocketv2kline)
> t(:codequote_subscribe)

```javascript
{
    "topic": "kline",
    "event": "sub",
    "params": {
        "symbol": "BTCUSDT",
        "klineType": "1m",
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# v2 does not support to subscribe mutiple symbols
# pass an interval
ws_spot.kline_v2_stream(
    handle_message, "ETHUSDT", "1h"
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "topic": "kline",
  "params": {
    "symbol": "BTCUSDT",
    "binary": "false",
    "klineType": "1m",
    "symbolName": "BTCUSDT"
  },
  "data": {
    "t": 1582001880000,
    "s": "BTCUSDT",
    "sn": "BTCUSDT",
    "c": "9799.4",
    "h": "9801.4",
    "l": "9798.91",
    "o": "9799.4",
    "v": "15.917433"
  }
}
```
t(:spot_websocket_kline_desc_v2)

t(:spot_public_websocket_frequency_near_realtime)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| topic | string | t(:spot_topic) |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| binary | string | t(:spot_binary) |
| klineType | string | t(:spot_kline_type) |
| symbolName | string | t(:spot_symbol) |
| t | number | t(:row_comment_resp_open_time) |
| s | string | t(:spot_symbol) |
| sn | string | t(:spot_symbol) |
| c | string | t(:spot_close)|
| h | string | t(:spot_high)|
| l | string | t(:spot_low)|
| o | string | t(:spot_open)|
| v | string | t(:spot_volume)|

### t(:websocketv2trade)
> t(:codequote_subscribe)

```javascript
{
    "topic": "trade",
    "params": {
        "symbol": "BTCUSDT",
        "binary": false
    },
    "event": "sub"
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# v2 does not support to subscribe mutiple symbols
ws_spot.trade_v2_stream(
    handle_message, "ETHUSDT"
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "topic": "trade",
  "params": {
    "symbol": "BTCUSDT",
    "binary": "false",
    "symbolName": "BTCUSDT"
  },
  "data": {
    "v": "564265886622695424",
    "t": 1582001735462,
    "p": "9787.5",
    "q": "0.195009",
    "m": true
  }
}
```

t(:spot_websocket_trade_desc_v2)

t(:spot_public_websocket_frequency_near_realtime)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| topic | string | t(:spot_topic) |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| binary | string | t(:spot_binary) |
| symbolName | string | t(:spot_symbol) |
| v | string | t(:spot_transactId) |
| t | number | t(:spot_time) |
| p | string | t(:spot_price) |
| q | string | t(:spot_quantity)|
| m | boolean | t(:spot_side) |

### t(:websocketv2bookticker)
> t(:codequote_subscribe)

```javascript
{
    "topic": "bookTicker",
    "event": "sub",
    "params": {
        "symbol": "BTCUSDT",
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# v2 does not support to subscribe mutiple symbols
ws_spot.book_ticker_v2_stream(
    handle_message, "ETHUSDT"
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "topic": "bookTicker",
  "params": {
    "symbol": "BTCUSDT",
    "binary": "false",
    "symbolName": "BTCUSDT"
  },
  "data": {
    "symbol": "BTCUSDT",
    "bidPrice": "9797.79",
    "bidQty": "0.177976",
    "askPrice": "9799",
    "askQty": "0.65",
    "time": 1582001830346
  }
}
```
t(:spot_websocket_ticker_desc_v2)

t(:spot_public_websocket_frequency_250_delay_300)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| topic | string | t(:spot_topic) |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| binary | string | t(:spot_binary) |
| symbolName | string | t(:spot_symbol) |
| bidPrice | string | t(:spot_buy_price) |
| bidQty | string | t(:spot_buy_qty) |
| askPrice | string | t(:spot_sell_price)|
| askQty | boolean | t(:spot_sell_qty) |
| time | member | t(:spot_timestamp2) |


### t(:websocketv2realtimes)
> t(:codequote_subscribe)

```javascript
{
    "topic": "realtimes",
    "event": "sub",
    "params": {
        "symbol": "BTCUSDT",
        "binary": false
    }
}
```

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
# v2 does not support to subscribe mutiple symbols
ws_spot.realtimes_v2_stream(
    handle_message, "ETHUSDT"
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
{
  "topic": "realtimes",
  "params": {
    "symbol": "BTCUSDT",
    "binary": "false",
    "symbolName": "BTCUSDT"
  },
  "data": {
    "t": 1582001616500,
    "s": "BTCUSDT",
    "o": "9736.5",
    "h": "9830.19",
    "l": "9455.71",
    "c": "9796.75",
    "v": "77211.561764",
    "qv": "740412516.91255711",
    "m": "0.0062"
  }
}
```
t(:spot_websocket_symbol_ticker_desc_v2)

t(:spot_public_websocket_frequency_near_realtime)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| topic | string | t(:spot_topic) |
| t(:row_parameter_symbol) | string | t(:spot_symbol) |
| binary | string | t(:spot_binary) |
| symbolName | string | t(:spot_symbol) |
| t | number | t(:spot_time) |
| s | string | t(:spot_symbol) |
| c | string | t(:spot_close)|
| h | string | t(:spot_high)|
| l | string | t(:spot_low)|
| o | string | t(:spot_open)|
| v | string | t(:spot_volume)|
| qv | string | t(:spot_quote_volume)|
| m | string | t(:spot_gains)|


## t(:privatetopics)

t(:spot_private_topics_auth_sub)

### t(:outboundAccountInfo)
> t(:spot_private_topics_auth_sub)

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    api_key="your api key",
    api_secret="your api secret",
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
ws_spot.outbound_account_info_stream(
    handle_message
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
[
    {
        "e":"outboundAccountInfo",
        "E":"1629969654753",
        "T":true,
        "W":true,
        "D":true,
        "B":[
            {
                "a":"BTC",
                "f":"10000000097.1982823144",
                "l":"0"
            }
        ]
    }
]
```

t(:outboundAccountInfo_desc)

t(:spot_public_websocket_frequency_realtime)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| e | string | t(:spot_event_type) |
| E | string | t(:spot_timestamp) |
| T | boolean | t(:spot_allow_trade) |
| W | boolean | t(:spot_allow_withdraw) |
| D | boolean | t(:spot_allow_deposit)|
| B | list | t(:spot_balance_change) |
| a | string | t(:spot_asset) |
| f | string | t(:spot_available_qty) |
| l | string | t(:spot_frozen_qty) |

### t(:executionReport)
> t(:spot_private_topics_auth_sub)

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    api_key="your api key",
    api_secret="your api secret",
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
ws_spot.execution_report_stream(
    handle_message
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
[
  {
    "e": "executionReport",      
    "E": "1499405658658",            
    "s": "ETHBTC",                 
    "c": "1000087761",               
    "S": "BUY",                    
    "o": "LIMIT",                  
    "f": "GTC",                    
    "q": "1.00000000",             
    "p": "0.10264410",             
    "X": "NEW",                    
    "i": "4293153",     
    "M": "0",             
    "l": "0.00000000",             
    "z": "0.00000000",             
    "L": "0.00000000",             
    "n": "0",                      
    "N": "BTC",                     
    "u": true,                     
    "w": true,                     
    "m": false,                    
    "O": "1499405658657",            
    "Z": "473.199",
    "A": "0",
    "C": false,
    "v": "0"              
  }
]
```
t(:executionReport_desc)

t(:spot_public_websocket_frequency_realtime)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| e | string | t(:spot_event_type) |
| E | string | t(:spot_event_timestamp) |
| s | string | t(:spot_symbol) |
| c | string | t(:spot_order_client_id) |
| S | string | t(:spot_side_private) |
| <a href="#order-type-type-ordertypes">o</a> | string | t(:spotType) |
| <a href="#timeinforce-timeinforce">f</a> | string | t(:row_comment_timeInForce) |
| q | string | t(:spot_quantity) |
| p | string | t(:spot_price) |
| <a href="#order-status-status">X</a> | string | t(:spotStatus) |
| i | string | t(:spot_order_id) |
| M | string | t(:spot_match_order_id) |
| l | string | t(:spot_recent_qty) |
| z | string | t(:spot_cumulative_qty) |
| L | string | t(:spot_recent_price) |
| n | string | t(:spot_fee) |
| N | string | t(:spot_fee_symbol) |
| u | boolean | t(:spot_is_normal) |
| w | boolean | t(:spot_is_working) |
| m | boolean | t(:spot_is_limit_maker) |
| O | string | t(:spot_order_create_timestamp) |
| Z | string | t(:spot_cumulative_trade) |
| A | string | t(:spot_match_account_id) |
| C | boolean | t(:spot_is_close) |
| v | string | t(:spot_leverage) |
| d | string | t(:spot_liquidation_type) |

### t(:ticketInfo)
> t(:spot_private_topics_auth_sub)

```python--pybit
from time import sleep
from pybit import spot
ws_spot = spot.WebSocket(
    test=True,
    api_key="your api key",
    api_secret="your api secret",
    ping_interval=30,  # the default is 30
    ping_timeout=10,  # the default is 10
    domain="bybit"  # the default is "bybit"
)
def handle_message(msg):
    print(msg)
ws_spot.ticket_info_stream(
    handle_message
)
while True:
    sleep(1)
```

> t(:codequote_responseExampleFormatAll)

```javascript
[
  {
    "e":"ticketInfo",
    "E":"1621912542359",
    "s":"BTCUSDT",
    "q":"0.001639",
    "t":"1621912542314",
    "p":"61000.0",
    "T":"899062000267837441",
    "o":"899048013515737344",
    "c":"1621910874883",
    "O":"899062000118679808",
    "a":"10043",
    "A":"10024",
    "m":true,
    "S":"BUY"
  }
]
```

t(:ticketInfo_para)

t(:spot_public_websocket_frequency_realtime)

<p class="fake_header">t(:responseparameters)</p>

|t(:column_parameter)|t(:column_type)|t(:column_comments)|
|:----- |:-----|----- |
| e | string | t(:spot_event_type) |
| E | string | t(:spot_event_timestamp) |
| s | string | t(:spot_symbol) |
| q | string | t(:spot_quantity) |
| t | string | t(:spot_timestamp) |
| p | string | t(:spot_price) |
| T | string | t(:spot_transactId) |
| o | string | t(:spot_order_id) |
| c | string | t(:spot_order_client_id) |
| O | string | t(:spot_match_order_id) |
| a | string | t(:spot_account_id) |
| A | string | t(:spot_match_account_id) |
| m | boolean | t(:spot_ticket_info_maker) |
| S | string | t(:spot_ticket_info_side) |
