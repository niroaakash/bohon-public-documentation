# Bohon API Documentation

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
  *   Get Incomplete Order Informations [check here](http://bohon.herokuapp.com/api/orders)

* ### Payment APIs:
  *   Create Payment [check here](http://bohon.herokuapp.com/api/order/pay/)
  *   Create Payment Wallet [check here](http://bohon.herokuapp.com/api/wallet/create/)
  *   Verify Payment [check here](http://bohon.herokuapp.com/api/order/pay/verify/)
  *   Save Wallet Information on Backend [check here](http://bohon.herokuapp.com/api/wallet/save-data/)


## API Documentation:

### New user Signup: 
Once This endpoint is called it will create the user with the provided Information from the request Body.

**required parameters in request body** : 
        
        {
                'username': <user_name>, 
                'first_name': <fist_name>, 
                'last_name': <last_name>, 
                'email': <email>, 
                'password': <password>, 
                'password2': <confirm_password>, 
                'user_type': <user_type> e.g <'Basic User' or 'Business User' or 'Co-Admin Editor'>
        }

### 3. Login:
Once this endpoint is called it will authorize the user with the *username* and *password* provided in the request body and is authorization is done successfully then it will send a ***JSON Web Token*** in response.

**required parameters in request body** : 
        
        {
                'username': <user_name>,
                'password': <password> 
        }
**response**:

        {
                'username': <user_name>,
                'password': <password> 
        }

### 4. Logout:
This endpoint is called to logout user from backend.

### 5. User Info: 
While calling this endpint the JSON Web Token should be passed in ***Authorization-Header*** in the format:

        Authorization: Bearer <token> 
If the token is valid then it will give the information of the currently logged user.

### 6. Incomplete / Live Order Info:
Once this endpoint is called it will give all the incomplete order details that the currently logged in user have made as response. The JSON Web Token also should be passed as the previously mentioned way.
This information is need to fill the Order Section of the Dashboard

### 7. Product Info: (optional):
Once this endpoint is called this will give all the Products details that the currently logged in user have ordered to transfer, as response. The JSON Web Token also should be passed as the previously mentioned way.
This information is need to fill the Products Section of the Dashboard.

### 8. Storing Products Calculating Price:
Once this endpoint is called this will store the product details on database with the data provided in *request body* and calculate the price.

### 9. Create Bohon Order API:
This endpoint is called to Initiate an Order. 

### 10. Process Bohon Order API:
This endpoint is called to Process an Initiated Order. 

### 11. Automated Order Generation (for business and co-admin users) API:
This endpoint is called by business and co-admin users to automate the ordering process. Just the user first should be a logged in user, after login user can call this APIs with the order details(not specified yet). 
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

### 12. Create Payment API (Under development): (frontend integration)
This endpoint is called to Initiate a payment on backend (through Razorpay). 

### 13. Create Payment Wallet API (Under development): (frontend integration)
This endpoint is called to automate the *Payment Customer creation* and *Payment Order Creation* process on backend (through Razorpay).

### 14. Verify Payment API:
This endpoint is called to Verify the payment, done through RazorPay Payment Gateway.
**required parameters in request body** : 
        
        {
                "razorpay_order_id": ,
                "razorpay_payment_id": , 
                "razorpay_signature": 
        }


### 15. Save Payment Wallet Data API:
This endpoint is called to Save the Payment wallet Information in Bohon Backend in order to avoid recreation of wallet everytime payment transfer done.
**required parameters in request body** : 
        
        {
                "cust_id": ,
                "order_id": ,
                "gstin": ,
                "payment_id": ,
                "amount": , 
        }
