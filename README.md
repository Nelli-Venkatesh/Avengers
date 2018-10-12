# Avengers

This library is used for transmit the data through the network channel securely between client and server with digital signature.In the server side i used IIS express as server and wrote restful services in .Net Web api and in client side i used angularjs service to http requests.currently it only supports GET and POST requests.

# usage

## in C#

###### For Token Generation

```
  token_gen.initialize();  // For Initializing the token generation library.
  token_gen.expiry_minutes = 30; // For setting lifespam of a token. 
  token_gen.addClaim("admin"); // For adding preveliges to the user in token. 
  token_gen.PRIMARY_MACHINE_KEY = ""; //These two machine keys for encryption of token and decryption this one must be 32 bytes length.
  token_gen.SECONDARY_MACHINE_KEY = ""; //Must be 16 bytes length.
  token_gen.addResponse("Status", "Success"); //adding additional response paramters to http response.
  token_gen.generate_token(); //To generate token based on above parameters.
```
###### Example

```
        [HttpPost]
        [Route("Token")]
        public IHttpActionResult Token()
        {
            token_gen.initialize();
            token_gen.expiry_minutes = 30;
            token_gen.addClaim("admin");
            token_gen.PRIMARY_MACHINE_KEY = "10101010101010101010101010101010";
            token_gen.SECONDARY_MACHINE_KEY = "1010101010101010";
            token_gen.addResponse("Status", "Success");

           return Ok(token_gen.generate_token());
        }
   ```     

###### For Token Verifications
```
  token_gen.AuthorizeClaim("admin"); // Only the added user can access this api. 
  string value = token_gen.Authorize(data); // returns the decrypted data
```
###### Example 

```
        [HttpPost]
        [Route("Encrypt_Check")]
        public IHttpActionResult Encrypt_Check(dynamic data)
        {

            token_gen.AuthorizeClaim("admin");
            string value = token_gen.Authorize(data);
            return Ok(value);
        }

        [HttpGet]
        [Route("Encrypt_Check_get")]
        public IHttpActionResult Encrypt_Check_get(string data)
        {

            token_gen.AuthorizeClaim("admin");
            string value = token_gen.Authorize(data);

            return Ok(value);
        }
```

## in Angularjs

###### Description

Module Name : Network
Controller Import Name : network_service

```
    var req = { Name: "Venkatesh Nelli", Type: "Admin" }; // JOSN Data
    
    ns.encrypt_post(URL, req, token, function (data) { // token which is generated in C# backend 
         console.log("From Response : " + data.data);
      },
      function (data) {
          console.log("Error Response : " + data.data);
    });

    ns.encrypt_get(URL, req, token, function (data) {
          console.log("From Response : " + data.data);
      },
      function (data) {
            console.log("Error Response : " + data.data);
      });

```

###### Example

```
(function () {

     var app = angular.module("myApp", ['Network']);

    app.controller("Home_Controller", ['$scope','network_service', home_ctrl]);

    function home_ctrl(scope, datacontext, ns) {


            var req = { Name: "Venkatesh Nelli", Type: "Admin" };

            ns.encrypt_post(URL, req, token, function (data) {
                console.log("From Response : " + data.data);
            },
                function (data) {
                    console.log("Error Response : " + data.data);
                });


            ns.encrypt_get(URL, req, token, function (data) {
                console.log("From Response : " + data.data);
            },
                function (data) {
                    console.log("Error Response : " + data.data);
                });

        }
        })();
        
```

## Additional Info

###### Expiry Methods
```
token_gen.expiry_days = 30 // token  expire duration in days 
token_gen.expiry_seconds = 30 // token  expire duration in seconds
token_gen.expiry_minutes = 30 // token  expire duration in minutes
token_gen.expiry_hours = 30 // token  expire duration in hours
token_gen.expiry_month = 30 // token  expire duration in month
token_gen.expiry_year = 30 // token  expire duration in year
```


