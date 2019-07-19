[![plusqo exchange platform](https://trading.plusqo.io/assets/frontend/img/logo_light.svg)](https://trading.plusqo.io)

# RESTful-API

PlusQO RESTful API for Traders - V1 / [ Base URL: `https://trading.plusqo.io/api/v1/` ]

### **Public API**

Does not require the use of authentication methods and is available via `GET` requests.
This API does not require authorization, and is accessible through the `GET` request.
In general, the URL for accessing the API is as follows `/api/v1/api_method/api_action?api_params` where api_method – this API method that is accessed, api_action – the action to be performed and api_params – incoming request parameters.

### **Private API**

Requires the use of an authorization and is available only by using a `POST` request and `GET` request.
To access this API requires authentication and you **must use `POST` methods**. API keys can be obtained in your account once you have registered to be a trader in [PlusQO Trading Exchange.](https://trading.plusqo.io)

__***Authorization***__
<br/>Authorization is done via sending two headers:

  1. **`key`** - The public key, that can be obtained from user’s account
  2. **`sign`** - `POST` data (param = val & param1 = val1 … & paramN = valN), signed by the secret key HMAC-SHA512, secret key is also obtained from user’s account


#### ***PHP example***

```php
    $api_url = "https://trading.plusqo.io/api/v1/trade/list";
    $mt = explode(' ', microtime());
    $NONCE = $mt[1] . substr($mt[0], 2, 6);
    $data = array('pair_id' => 1, 'nonce' => $NONCE);
    $post_data = http_build_query($data, '', '&');
    $sign = hash_hmac('sha512', $post_data, $privateKey);
    $headers = array("Key: $publicKey", "Sign: $sign");
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_URL, $api_url);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
    $result = curl_exec($ch);
    //handle with $result here...
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

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "pagination": {
    "current_page": 1,
    "items_per_page": 10,
    "total_items": 100,
    "total_pages": 10
  },
  "response": {
    "entities": [
      {
        "currency_id": 1,
        "iso": "BTC",
        "name": "Bitcoin"
      }
    ]
  }
}
```
</details>



### 2. **`GET`&nbsp;&nbsp;/currency/info** (Get currency by id) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
currency\_id | integer | Currency id to find (*required*) |  | 

Sample call : `https://trading.plusqo.io/api/v1/currency/info/?currency_id=1`

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "response": {
    "entity": {
      "currency_id": 1,
      "iso": "BTC",
      "name": "Bitcoin"
    }
  }
}
```
</details>



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

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "pagination": {
    "current_page": 1,
    "items_per_page": 10,
    "total_items": 100,
    "total_pages": 10
  },
  "response": {
    "entities": [
      {
        "trade_id": 1,
        "pair_id": 1,
        "type": "buy",
        "price": 8200,
        "volume": 1,
        "created": 1529515521
      }
    ]
  }
}
```
</details>


### 4. **`GET`&nbsp;&nbsp;/trade/info** (Get trade log by id) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
trade\_id | integer | Trade id to find (*required*) |  | 

Sample call : `https://trading.plusqo.io/api/v1/trade/info/?trade_id=1`

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "response": {
    "entity": {
      "trade_id": 1,
      "pair_id": 1,
      "type": "buy",
      "price": 8200,
      "volume": 1,
      "created": 1529515521
    }
  }
}
```
</details>


### 5. **`GET`&nbsp;&nbsp;/pair/list** (Get currency pairs list) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
order\_by | string | Field to order by | pair\_id | pair\_id, currency\_id\_from, currency\_id\_to
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

Sample call : `https://trading.plusqo.io/api/v1/pair/list`

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "pagination": {
    "current_page": 1,
    "items_per_page": 10,
    "total_items": 100,
    "total_pages": 10
  },
  "response": {
    "entities": [
      {
        "pair_id": 1,
        "currency_id_from": 1,
        "currency_id_to": 2,
        "filters": {
          "price": {
            "min": "0.00000010",
            "step": "0.00000010"
          },
          "amount": {
            "min": "0.00000010",
            "step": "0.00000010"
          }
        }
      }
    ]
  }
}
```
</details>


### 6. **`GET`&nbsp;&nbsp;/pair/info** (Get currency pair by id) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id to find (*required*) |  | 

Sample call : `https://trading.plusqo.io/api/v1/pair/info/?pair_id=1`

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "response": {
    "entity": {
      "pair_id": 1,
      "currency_id_from": 1,
      "currency_id_to": 2,
      "filters": {
        "price": {
          "min": "0.00000010",
          "step": "0.00000010"
        },
        "amount": {
          "min": "0.00000010",
          "step": "0.00000010"
        }
      }
    }
  }
}
```
</details>


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

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "pagination": {
    "current_page": 1,
    "items_per_page": 10,
    "total_items": 100,
    "total_pages": 10
  },
  "response": {
    "entities": [
      {
        "pair_id": 1,
        "min": 1,
        "max": 100.234,
        "avg": 65.22452,
        "vol": 987.1
      }
    ]
  }
}
```
</details>


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

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "response": {
    "entity": {
      "pair_id": 1,
      "pair_name": "BTC/EUR",
      "last_price": 8203.98,
      "min": 1,
      "max": 8330.23,
      "avg": 8208.22,
      "vol": 99887.1
    }
  }
}
```
</details>


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

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "pagination": {
    "current_page": 1,
    "items_per_page": 10,
    "total_items": 100,
    "total_pages": 10
  },
  "response": {
    "entities": [
      {
        "pair_id": 1,
        "pair_name": "BTC/EUR",
        "last_price": 8203.98,
        "max_bid": 8200,
        "min_ask": 8210.23,
        "vol": 99887.1
      }
    ]
  }
}
```
</details>


### 10. **`GET`&nbsp;&nbsp;/orderbook/info** (Get orderbook info) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id to find (*required*)|  | 
date\_from | integer | Timestamp to start | 0 | 
date\_to | integer | Timestamp to end | current timestamp | 
type | string | type to find | | bid, ask

Sample call : `https://trading.plusqo.io/api/v1/orderbook/info?pair_id=1`

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "response": {
    "entities": {
      "asks": [
        {
          "amount": 0.30837754,
          "price": 7473.89
        }
      ],
      "bids": [
        {
          "amount": 0.30837754,
          "price": 7473.89
        }
      ]
    }
  }
}
```
</details>


### 11. **`GET`&nbsp;&nbsp;/orderbook/ticker** (Get orderbook ticker) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id to find |  | 

Sample call : `https://trading.plusqo.io/api/v1/orderbook/ticker?pair_id=1`

<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "response": {
    "entities": [
      {
        "pair_id": 1,
        "pair_name": "BTC/EUR",
        "ask": {
          "price": 8700,
          "amount": 1
        },
        "bid": {
          "price": 8600,
          "amount": 2
        }
      }
    ]
  }
}
```

</details>


## **`Private`** calls requiring obtaining API key pair

### 1. **`POST`&nbsp;&nbsp;/trade/list** (Get trade logs list) 

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
pair\_id | integer | Currency pair id |  | 
type | string | type to find |  | buy, sell
date\_from | integer | Timestamp to start | -1 day | 
datef\_to | integer | Timestamp to end | current timestamp | 
order\_by | string | Field to order by | trade\_id | trade\_id, pair\_id, type, price, volume, fee, created
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

<details>
 <summary>Sample call in PHP</summary>
 
```php
    $api_url = "https://trading.plusqo.io/api/v1/trade/list";
    $mt = explode(' ', microtime());
    $NONCE = $mt[1] . substr($mt[0], 2, 6);
    $data = array('pair_id' => 1, 'nonce' => $NONCE);
    $post_data = http_build_query($data, '', '&');
    $sign = hash_hmac('sha512', $post_data, $privateKey);
    $headers = array("Key: $publicKey", "Sign: $sign");
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_URL, $api_url);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
    $result = curl_exec($ch);
    //handle with $result here...
	
```
</details>


<details>
 <summary>Sample Response (application/json)</summary>
 
```javascript
{
  "errors": {
    "field": "Error text for input named field"
  },
  "pagination": {
    "current_page": 1,
    "items_per_page": 10,
    "total_items": 100,
    "total_pages": 10
  },
  "response": {
    "entities": [
      {
        "trade_id": 1,
        "pair_id": 1,
        "is_buyer": 0,
        "is_seller": 1,
        "type": "buy",
        "fee": 3,
        "price": 8200,
        "volume": 1,
        "created": 1529515521
      }
    ]
  }
}
```
</details>

