[![plusqo exchange platform](https://trading.plusqo.io/assets/frontend/img/logo_light.svg)](https://trading.plusqo.io)

# RESTful-API

PlusQO RESTful API for Traders - V1 / [ Base URL: `https://trading.plusqo.io/api/v1/` ]

### **Public API**

Does not require the use of authentication methods and is available via GET requests.
This API does not require authorization, and is accessible through the GET request.
In general, the URL for accessing the API is as follows `/api/v1/api_method/api_action?api_params` where api_method – this API method that is accessed, api_action – the action to be performed and api_params – incoming request parameters.

### **Private API**

Requires the use of an authorization and is available only by using a POST request and GET request.
To access this API requires authentication and you must use POST methods. API keys can be obtained in your account.

__***Authorization***__
<br/>Authorization is done via sending two headers:

  1. **`key`** - The public key, that can be obtained from user’s account
  2. **`sign`** - POST data (param = val & param1 = val1 … & paramN = valN), signed by the secret key HMAC-SHA512, secret key is also obtained from user’s account


#### ***PHP example***

```php
    $postData = http_build_query($data);
    $sign = hash_hmac('sha512', $postData, $privateKey);
    $headers = array("Key: $publicKey", "Sign: $sign");
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
```

## **`Public`** calls available for all without registration


### 1. **`GET`&nbsp;&nbsp;/currency/list** (Get currencies list) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
order\_by | string | Field to order by | currency\_id | iso, name, currency\_id
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

Sample call : `https://trading.plusqo.io/api/v1/currency/list?order_by=name`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10,<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"responses": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entities": \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"iso" : "BTC",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name" : "Bitcoin",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"currency_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\]<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 2. **`GET`&nbsp;&nbsp;/currency/info** (Get currency by id) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
currency\_id | integer | Currency id to find (*required*) |  | 

Sample call : `https://trading.plusqo.io/api/v1/currency/info/?currency_id=1`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"response": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entity": <br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"iso" : "BTC",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name" : "Bitcoin",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"currency_id" : 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 3. **`GET`&nbsp;&nbsp;/trade/list** (Get trade logs list) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id |  | 
type | string | type to find |  | buy, sell
date\_from | integer | Timestamp to start | -1 day | 
date\_to | integer | Timestamp to end | current timestamp | 
order\_by | string | Field to order by (*required*) | trade\_id | trade\_id, pair\_id, type, price, volume, fee, created 
order\_direction | string | Direction to order by (*required*) | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) (*required*) | 100 | 
page | integer | current page (*required*) | 1 | 

Sample call : `https://trading.plusqo.io/api/v1/trade/list?pair_id=1`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"responses": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entities": \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"trade_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type" : "buy",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"price" : 8200,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"volume" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"created" : 1529515521<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\]<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 4. **`GET`&nbsp;&nbsp;/trade/info** (Get trade log by id) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
trade\_id | integer | Trade id to find (*required*) |  | 

Sample call : `https://trading.plusqo.io/api/v1/trade/info/?trade_id=1`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"response": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entity": <br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"trade_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type" : "buy"<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"price" : 8200<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"volume" : 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"created" : 1529515521<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 5. **`GET`&nbsp;&nbsp;/pair/list** (Get currency pairs list) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
order\_by | string | Field to order by | pair\_id | pair\_id, currency\_id\_from, currency\_id\_to
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

Sample call : `https://trading.plusqo.io/api/v1/pair/list`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"responses": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entities": \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"currency_id_from" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"currency_id_to" : 2,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"filters" : {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"price" : {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"step" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"amount" : {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"step" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\]<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 6. **`GET`&nbsp;&nbsp;/pair/info** (Get currency pair by id) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id to find (*required*) |  | 

Sample call : `https://trading.plusqo.io/api/v1/pair/info/?pair_id=1`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"response": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entity": \{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"currency_id_from" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"currency_id_to" : 2,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"filters" : {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"price" : {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"step" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"amount" : {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"step" : "0.00000010",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\}<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 7. **`GET`&nbsp;&nbsp;/stats/marketdepth** (Get market depth stats dependant on trade logs) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id |  | 
date\_from | integer | Timestamp to start | -1 month | 
date\_to | integer | Timestamp to end | current timestamp | 
order\_by | string | Field to order by | pair\_id | pair_id, min, max, avg, vol
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

Sample call : `https://trading.plusqo.io/api/v1/stats/marketdepth`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"responses": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entities": \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"max" : 100.234,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"avg" : 65.22452,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"vol" : 987.1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\]<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 8. **`GET`&nbsp;&nbsp;/stats/marketdepthfull** (Get market depth stats dependant on trade logs) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id |  | 
date\_from | integer | Timestamp to start | -1 day | 
date\_to | integer | Timestamp to end | current timestamp | 
order\_by | string | Field to order by | pair\_id | pair_id, min, max, avg, vol
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

Sample call : `https://trading.plusqo.io/api/v1/stats/marketdepthfull`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"responses": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entities": \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_name" : "BTC/EUR",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"last_price" : 8203.98,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"max" : 8330.23,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"avg" : 8208.22,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"vol" : 99887.1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\]<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 9. **`GET`&nbsp;&nbsp;/stats/marketdepthbtcav** (Get market depth stats dependant on trade logs) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id |  | 
date\_from | integer | Timestamp to start | -1 day | 
date\_to | integer | Timestamp to end | current timestamp | 
order\_by | string | Field to order by | pair\_id | pair_id, min, max, avg, vol
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

Sample call : `https://trading.plusqo.io/api/v1/stats/marketdepthfull`

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"responses": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entities": \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_name" : "BTC/EUR",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"last_price" : 8203.98,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"max_bid" : 8200,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min_ask" : 8210.23,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"vol" : 99887.1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\]<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error


### 10. **`GET`&nbsp;&nbsp;/orderbook/info** (Get orderbook info) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id to find (*required*)|  | 
date\_from | integer | Timestamp to start | 0 | 
date\_to | integer | Timestamp to end | current timestamp | 
type | string | type to find | | bid, ask

Sample call : `https://trading.plusqo.io/api/v1/orderbook/info?pair_id=1

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;"responses": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"entities": \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_id" : 1,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"pair_name" : "BTC/EUR",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"last_price" : 8203.98,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"max_bid" : 8200,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"min_ask" : 8210.23,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"vol" : 99887.1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\]<br/>&nbsp;&nbsp;&nbsp;}<br/>}
500 |  Server error
