# RESTful-API
PlusQO RESTful API for Traders - V1 / [ Base URL: https://trading.plusqo.io/api/v1/ ]

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


* Get currencies list **`GET`&nbsp;&nbsp;/currency/list**

Parameter

Name | Type | Description | Default value | Available values 
--- | --- | --- | --- | ---
order\_by | string | Field to order by | currency\_id | iso, name, currency\_id
order\_direction | string | Direction to order by | asc | asc, desc
items\_per\_page | integer | How many items to show per page (min: 1, max: 100) | 100 | 
page | integer | Current page | 1 | 

Sample Responses

Code | Description 
--- | --- 
200 | {<br/>&nbsp;&nbsp;&nbsp;"errors": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"field": "Error text for input named field"<br/>&nbsp;&nbsp;&nbsp;}, <br/>&nbsp;&nbsp;&nbsp;"pagination": {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"current_page": 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"items_per_page": 10<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_items": 100<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_pages": 10<br/>&nbsp;&nbsp;&nbsp;},<br/>}
500 |  Server error
