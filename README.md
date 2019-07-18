# RESTful-API
PlusQO RESTful API for Traders - V1 / [ Base URL: https://trading.plusqo.io/api/v1/ ]

### **Public API**

Does not require the use of authentication methods and is available via GET requests.
This API does not require authorization, and is accessible through the GET request.
In general, the URL for accessing the API is as follows `/api/v1/api_method/api_action?api_params` where api_method – this API method that is accessed, api_action – the action to be performed and api_params – incoming request parameters.

### **Private API**

Requires the use of an authorization and is available only by using a POST request and GET request.
To access this API requires authentication and you must use POST methods. API keys can be obtained in your account.

**Authorization**
<br/>Authorization is done via sending two headers:
