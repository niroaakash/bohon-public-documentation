# Bohon API Integration

## Available End points:

* ### Authorization APIs: 
  *   SignUp [check here](http://bohon.herokuapp.com/api/auth/register/)
  *   SignUp with Google [check here](http://bohon.herokuapp.com/api/auth/google/)
  *   Logout [click here](http://bohon.herokuapp.com/api/auth/logout/)
  *   Login [check here](http://bohon.herokuapp.com/api/auth/login/)
  *   User Info [check here](http://bohon.herokuapp.com/api/auth/user)

* ### Product APIs:
    *  Get Saved Product Informations [check here](http://bohon.herokuapp.com/api/products)
    *   Store Product Info and Calculate Price [check here](http://bohon.herokuapp.com/api/products/store)

* ### Order APIs:
  *   Create Bohon Order [check here](http://bohon.herokuapp.com/api/order/create/)
  *   Process Bohon Order [check here](http://bohon.herokuapp.com/api/order/process/)
  *   Generate Bohon Order (**Automation**) [check here](http://bohon.herokuapp.com/api/order/generate/)
  *   Get Order Informations [check here](http://bohon.herokuapp.com/api/orders)

* ### Payment APIs:
  *   Create Payment [check here](http://bohon.herokuapp.com/api/order/pay/)
  *   Create Payment Wallet [check here](http://bohon.herokuapp.com/api/wallet/create/)
  *   Verify Payment [check here](http://bohon.herokuapp.com/api/order/pay/verify/)
  *   Save Wallet Information on Backend [check here](http://bohon.herokuapp.com/api/wallet/save-data/)


#
# API Documentation:

## New user Signup: **/api/auth/register/** 
    POST: https://bohon.herokuapp.com/api/auth/register/
Once This endpoint is called it will create the user with the provided Information from the request Body.

**required parameters in request body** : 
        
    {
        "username": <user_name>, 
        "first_name": <fist_name>, 
        "last_name": <last_name>, 
        "email": <email>, 
        "password": <password>, 
        "password2": <confirm_password>, 
        "user_type": <user_type> e.g <"BASIC" or "BUSINESS" or "CO-ADMIN">
    }
    
**response**:

    {
        "user": {
            "id": <id>,
            "username": <given_user_name>,
            "email": <given_email>,
            "user_type": <user_type>
        },
        "token": <random_token>,
        "message": <message> e.g <"User Created Successfully">,
        "status code": <status_code> e.g <201>
    }

## Login: **/api/auth/login/**:
    POST: https://bohon.herokuapp.com/api/auth/login/
Once this endpoint is called it will authorize the user with the *username* and *password* provided in the request body and is authorization is done successfully then it will send a ***JSON Web Token*** in response.

**required parameters in request body** : 
        
    {
        "username": <user_name>,
        "password": <password> 
    }

**response**:

    {
        "success": <boolean> e.g <True or False>,
        "status code": <status_code> e.g <200>,
        "message": <message> e.g <"User logged in  successfully">,
        "user": {
            "id": <id>,
            "username": <given_user_name>,
            "email": <given_email>,
            "user_type": <user_type>
        },
        "token": <JSON_web_token> 
    }

## Logout: **/api/auth/logout/**
    GET: https://bohon.herokuapp.com/api/auth/logout/
This endpoint is called to logout user from Bohon.

## User Info: 
    GET: http://bohon.herokuapp.com/api/auth/user
While calling this endpint the JSON Web Token should be passed in ***Authorization-Header*** in the following format in the request header. If the token is valid then it will give the information of the currently logged user.

**request header**:

    Authorization: Bearer <JSON_web_token> 

**response**:

    {
        "id": <id>,
        "username": <given_user_name>,
        "email": <given_email>,
        "user_type": <user_type>
    }

## Automated Order Generation API (for business and co-admin users): **/api/order/generate/**
    POST: http://bohon.herokuapp.com/api/order/generate/
This endpoint is called by business and co-admin users to automate the ordering process. Just the user first should be a logged in user, after login user can call this APIs with the order details in the request body. The JSON Web Token also should be passed as the previously mentioned way in **authorization header**.

**request header**:

    Authorization: Bearer <JSON_web_token> 

**required parameters in request body** : 
        
    {
        "pickup": {
                "location":"delhi" ,
                "vendor_name": "sandipan",
                "contact_no": 1231
        },
        "drop": {
                "location":"Kolkata" ,
                "vendor_name": "swiggy",
                "contact_no": 4321
        },
        "product_weight": 15
    }

## All Order Info: **/api/orders**
    GET: http://bohon.herokuapp.com/api/orders
Once this endpoint is called it will give all the order details that the currently logged in user have made till date. The JSON Web Token also should be passed as the previously mentioned way in **authorization header**.

**request header**:

    Authorization: Bearer <JSON_web_token> 
