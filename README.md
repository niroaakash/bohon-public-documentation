# Bohon API 


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
                "location": <pickup_location> ,
                "vendor_name": <pickup_vendor's_name>,
                "contact_no": <pickup_vendor's_contact_no>
        },
        "drop": {
                "location":<drop_location> ,
                "vendor_name": <drop_vendor's_name>,
                "contact_no": <drop_vendor's_contact_no>
        },
        "product_weight": <package_weight>
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

## All Order Info: **/api/orders**
    GET: http://bohon.herokuapp.com/api/orders
Once this endpoint is called it will give all the order details that the currently logged in user have made till date. The JSON Web Token also should be passed as the previously mentioned way in **authorization header**.

**request header**:

    Authorization: Bearer <JSON_web_token> 

