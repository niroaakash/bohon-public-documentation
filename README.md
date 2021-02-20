# Bohon API 
Bohon API Integration for Business and Co-Admin Users. 

This documentation is mainly focused on ***integrating the Automation process***.

# API Documentation:

## New user Signup: **/api/auth/register/** 
POST: https://bohonapi-staging.herokuapp.com/api/auth/register/

Once This endpoint is called it will create the user with the provided Information from the request Body.

**required parameters in request body** : 
        
    {
        "username": <string:user_name>, 
        "first_name": <string:fist_name>, 
        "last_name": <string:last_name>, 
        "email": <string:email>, 
        "password": <string:password>, 
        "password2": <string:confirm_password>, 
        "user_type": <string:user_type> e.g <"BASIC" or "BUSINESS" or "CO-ADMIN">
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
POST: https://bohonapi-staging.herokuapp.com/api/auth/login/

Once this endpoint is called it will authorize the user with the *username* and *password* provided in the request body and is authorization is done successfully then it will send a ***JSON Web Token*** in response.

**required parameters in request body** : 
        
    {
        "username": <string:user_name>,
        "password": <string:password> 
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
GET: https://bohonapi-staging.herokuapp.com/api/auth/logout/

This endpoint is called to logout user from Bohon.

## User Info: 
GET: https://bohonapi-staging.herokuapp.com/api/auth/user

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
POST: https://bohonapi-staging.herokuapp.com/api/order/generate/

This endpoint is called by business and co-admin users to automate the ordering process. Just the user first should be a logged in user, after login user can call this APIs with the order details in the request body. The JSON Web Token also should be passed as the previously mentioned way in **authorization header**.

**request header**:

    Authorization: Bearer <JSON_web_token> 

**required parameters in request body** : 
        
    {
        "vendor": {
            "location": <string: vendor's_location>,
            "vendor_name": <string: vendor's_name>,
            "contact_no": <integer: vendor's_contact_no>
        },
        "customer": {
            "location":<string: customer'slocation> ,
            "vendor_name": <string: customer's_name>,
            "contact_no": <integer: customer's_contact_no>
        },
        "product_weight": <package_weight>,
        "payment_mode": <string:payment_mode> e.g <"COD" or "POD" or "ONLINE">
    }

**sample request body** : 
        
    {
        "vendor": {
            "location":"delhi" ,
            "name": "sandipan",
            "contact_no": 1231
        },
        "customer": {
            "location":"Kolkata" ,
            "name": "swiggy",
            "contact_no": 4321
        },
        "product_weight": 15,
        "payment_mode": "COD"
    }


**sample response 1** : 
 
    {
        "success": true,
        "delivery_response": {
            "delivery_partner_response": {
                [
                    <delevery_partner_responses>
                ]
            }
        },
        "message": "Order processed successfully",
        "status": 200,
        "order_details": {
            "id": 76,
            "order_id": "bohon_order_uxmUAeFYVEdVZNEetCls",
            "date_ordered": "2021-02-04T07:43:46.767149Z",
            "is_complete": true,
            "transaction_id": "1612428626.914945",
            "order_option": null,
            "calculated_price": null,
            "is_paid": false,
            "customer": 3,
            "delivery_partner_data": 3,
            "poc_data": 2
        }
    }

**sample response 2** : 

    {
        "success": false,
        "message": "Failed to create order",
        "status": 400
    }

## Get All Order Information for a Logged In user: **/api/orders**
GET: https://bohonapi-staging.herokuapp.com/api/orders

Once this endpoint is called it will give all the order details that the currently logged in user have made till date. The JSON Web Token also should be passed as the previously mentioned way in **authorization header**.

**request header**:

    Authorization: Bearer <JSON_web_token> 

