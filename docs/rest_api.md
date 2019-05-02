REST API
========

All parts of Matohmat communicate through a common JSON based REST API.
Here is the documentation of it, its ment for the ones who wants to program a client, or change the server.

The rest api on the server side is implemented using the [spring rest](https://docs.spring.io/spring/docs/current/spring-framework-reference/index.html).
[Here](https://spring.io/guides/gs/rest-service/) is a tutorial about how to write RESTful Apps with spring.


----
# Admin

__Endpoint: `/admins`__

#### Query all admins
`GET /v1/admins`

This request requires Admin privileges.

- `count=<number>` number of items per page. If this parameter is not given it will return all admins in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `onlyAvailable=<true/false>` if true will only return admins that are available. If false or not given it will return all admins in database.


__Reply__


```
[
   {
      "id":<admin id as long>,
      "user_id":<user id as long>,
      "balance":<balance in cent as integer>,
      "last_seen":<last login date as string>,
      "available":<is user available as boolean>,
      "user_name":<admin username>,
      "email":<email address>
   },
   <furhter admins>
]
```
----
#### Query one specific admin
`GET /v1/admins/<admin_id>`

This request requires Admin privileges.

__Reply__
```
{
   "id":<admin id as long>,
   "user_id":<user id as long>,
   "balance":<balance in cent as integer>,
   "last_seen":<last login date as string>,
   "available":<is user available as boolean>,
   "user_name":<admin username>,
   "email":<email address>
}
```
----
#### Query the loged in admin

`GET /v1/admins/me`

This request requires Admin privileges.

It will return the user represented by the basic auth credentials.

__Reply__
```
{
    "id":<admin id as long>,
    "user_id":<user id as long>,
    "balance":<balance in cent as integer>,
    "last_seen":<last login date as string>,
    "available":<is user available as boolean>,
    "user_name":<admin username>,
    "email":<email address>
}
```
----
#### Create a new admin

This request requires Admin privileges.

Every existing admin can create new admins.

`POST /v1/admins`

__Request__

```
{
   "user_name":<admin username>,
   "password":<admin password>,
   "email":<email address>
}
```

----

#### Change an admin

`PATCH /admins/<admin id>`

This request requires Admin privileges.

__Request__
```
{
   "user_name":<admin username>,
   "password":<admin password>,
   "email":<email address>
}
```

----
#### Delete an admin

`DELETE /v1/admins/<admin id>`

This request requires Admin privileges.

----
# User

__Endpoint: `/users`__

#### Query all users

`GET /v1/users`

This request requires Admin privileges.

- `count=<number>` number of items per page. If this parameter is not given it will return all users in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `onlyAvailable=<true/false>` if true will only return users that are available. If false or not given it will return all users in database.

__Reply__

```
[
   {
       "id":<user id as long>,
       "balance":<balance in cent as integer>,
       "last_seen":<last login date as string>,
       "available":<is user available as boolean>,
       "name":<name of the user>
   },
   <more users>
]
```

----
#### Query one specific user

`GET /v1/users/<user_id>`

This request requires Admin privileges.

__Reply__

```
{
    "id":<user id as long>,
    "balance":<balance in cent as integer>,
    "last_seen":<last login date as string>,
    "available":<is user available as boolean>,
    "name":<name of the user>
}
```

----
#### Query the current loged in user

`GET /v1/users/me`

This will require a user to send a basic auth header with his credentials.
It will return the user represented by the basic auth credentials.

__Reply__

```
{
    "id":<user id as long>,
    "balance":<balance in cent as integer>,
    "last_seen":<last login date as string>,
    "available":<is user available as boolean>
}
```

----
#### Create new user

`POST /v1/user`

A create new user may be sent without auth header, however if no basic auth header is given it requires
that the request contains a valid client key. These keys are used to permit privileged clients to create new users.
If an admin auth header is given the `client_key` can be empty or left out, and should not even be used here.
Regular users are not permitted to register new users. Only admins or priviliged clients.

The name of the user will be auto generated.

The body of the post must contain this data:
```
{
   "client_key":<key of the client priviliged to create new users. Can be left out if adbmin basic auth is given>,
   "auth_hash":<first 25 bytes of the auth hash (sha256) generated from the user id>
}
```

Example:
```
{
   "client_key":"09d374ebf788ca96d5c5ad8cfc778317d8efa692766accc609eb5932ccf19c94",
   "auth_hash":"6d78392a5886177fe5b86e585"
}
```

----
#### Update a user

`PATCH /v1/user/<user_id>`

I can not setup a user name through a create since the matomat does not support it (if it does later, you may sent a PATCH)
directly after you created a user. The unique name of a user is ment to be used for a potional smarthphone app where
I would be able to send money to other users without having to know their id.

The body of the post must contain this data:
```
{
   "auth_hash":<first 25 bytes of the auth hash (sha256) generated from the user id>,
   "name":<the unique user name>
}
```

Example:
```
{
   "auth_hash":"09d374ebf788ca96d5c5ad8cfc778317d8efa692766accc609eb5932ccf19c94",
   "name":"HansPeter"
}
```

----
#### Delete an user

`DELETE /v1/users/<admin id>`

This request requires Admin privileges.

----

# Product

__Endpoint: `/products`__

`GET /v1/products`

- `onlyAvailable=<true/false>` if true will only return products that are available. If false or not given it will return all products in database.
  Default value for this is false

__Reply__
```
[
   {
      "id":<product id as long>,
      "price":<product price in cent as int>,
      "valid_from":<date since current price is valid as string>,
      "name":<product name as string>,
      "thumbnail":<url of image thumbnail as string>,
      "reorder_point":<reorder point as int>,
      "barcode":<barcode of the product as string>,
      "items_per_crate":<items or bottles per crate>,
      "stock":<amount of items currently available>
   },
   <more products>
}
```
----

#### Query for specific product

`GET /v1/products/<product_id>`

__Reply__
```
{
   "id":<product id as long>,
   "price":<product price in cent as int>,
   "valid_from":<date since current price is valid as string>,
   "name":<product name as string>,
   "thumbnail":<url of image thumbnail as string>,
   "reorder_point":<reorder point as int>,
   "barcode":<barcode of the product as string>,
   "items_per_crate":<items or bottles per crate>,
   "stock":<amount of items currently available>
}
```

----
#### Add product

`POST /v1/products`

This request requires Admin privileges.
Here not only the product, but also the product info will be given,
since this has to be created at the same time.

__Request__

```
{
   "price":<product price in cent as int>,
   "name":<product name as string>,
   "thumbnail":<url of image thumbnail as string>,
   "reorder_point":<reorder point as int>,
   "barcode":<barcode of the product as string>,
   "items_per_crate":<items or bottles per crate>,
}   
```
----

#### Update Product

`PATCH /v1/products/<id>`

This request requires Admin privileges.

__Request__

```
{
   "price":<product price in cent as long>,
   "name":<product name as string>,
   "thumbnail":<url of image thumbnail as string>,
   "reorder_point":<reorder point as int>,
   "barcode":<barcode of the product as string>,
   "is_available":<is available as boolean>,
   "items_per_crate":<items or bottles per crate>
}   
```

----

# Transaction

__Endpoint: `/v1/transactions`__

There are 6 different types of transactions if a `type` is requested or given it can only be one of these values:
`purchse/deposit/withdraw/transfer/order/all`.

#### Query transactions

`GET /v1/transactions`

If this request is made by a regular user it will only display the transactions of the current user.

If specific transactions are picked they will lock different from how they look like when you query for all of them.
So make sure to check `transaction_type` before parsing the rest of the data. So for example, if 
a transaction is of type `order` or `purchase`it will also contain an attribute `products` which will be a list
of products that where ordered or soled.  
On top of this an order is a special kind of purchase, where there is not only an `amount`, by means the value of the sum of the price
of all products, but also a `buy_cost`, which is the price we paied at the marched for the products.
So the `amount` will be the price for which we sell the products, and `buy_cost` will be the price we buy them with.
The difference between `buy_cost` and `amount` will then be our __income__.

If `all` is entered as transaction type only the head of the transaction will be transmitted, by means only
the `id`, `date`, `sender` `receiver` and `transaction_type` will be visible. This is due to a simple and more efficient
implementation of the database.

- `count=<number>` number of items per page. If this parameter is not given it will return 100 items of the first page.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `user=<user_id>` get all transactions of the given user. __Atention__ this requires admin privileges. made.
- `type=<purchase/deposit/withdraw/transfer/order/all>` this will only show the transactions of a specific type.
  If left out or set to `all` it will show all the transactions.

__Request__
`GET /v1/transactions`  
__Reply__
```
[
    {
        "id":<transaction id as long>,
        "date":<transaction date as string>,
        "sender":<user id of the sender as long>,
        "receiver":<user id of the receiver as long>,
        "amount":<amount of money transfered in cent as long>,
        "transaction_type":<type of the transaction as string>
    },
    <etc>
]
```

#### Query a transfer

__CAUTION__ this may only return transactions of the current user (if not admin). Return a 404 if the transaction
exists but does not belong the the user currently logged in.

A transfer will look like a transaction without more details. A transfer can be of type

- `deposid` if someone pays money into the Matohmat system
- `withdraw` if someone takes away money form the Matohmat system (only possible for admins)
- `transfer` if someone sends money to someone else

__Request__
`GET /v1/transactions/<transaction_id>`  
__Response__
```
{
    "id":<transaction id as long>,
    "date":<transaction date as string>,
    "sender":<user id of the sender as long>,
    "receiver":<user id of the receiver as long>,
    "amount":<amount of money transfered in cent as long>,
    "transaction_type":<type of the transaction as string>
}
```

#### Query a purchase

__CAUTION__ this may only return transactions of the current user (if not admin). Return a 404 if the transaction
exists but does not belong the the user currently logged in.

Purchases have an extra data value called `purchases` which contains a list of products, and how much of them got bought.

__Request__
`GET /v1/transactions/<transaction_id>`  
__Response__
```
{
    "id":<transaction id as long>,
    "date":<transaction date as string>,
    "sender":<user id of the sender as long>,
    "receiver":<user id of the receiver as long>,
    "amount":<amount of money transfered in cent as long>,
    "transaction_type":"purchase"
    "purchased": [
        {
           "product":<product id as int>,
           "amount":<amount of this bought as int>
        },
        <etc>
    ]
}
```
#### Query an order

__CAUTION__ this may only return transactions of the current user (if not admin). Return a 404 if the transaction
exists but does not belong the the user currently logged in.

Order are special kind of purchases they not only contain a list of `purchases` but also a `buy_cost` which is the
price for how much the product got bought for.

__Request__
`GET /v1/transactions/<transaction_id>`  
__Response__
```
{
   "id":<transaction id as long>,
   "date":<transaction date as string>,
   "sender":<user id of the sender as long>,
   "receiver":<user id of the receiver as long>,
   "amount":<amount of money transfered in cent as long>,
   "transaction_type":"order",
   "admin":<id of the admin who created the order>,
   "purchased": [
      {
          "product_info":<product info id as int>,
          "amount":<amount of this bought as int>,
          "price_per_unit":<price for which one piece was sold by  then>
      },
      <etc>
   ]
}
```

----
#### Make new transfer

If you want to make a transfer, a user can not change the sender this a sender
does not have to be given for a user. However only admins can change both
sender and receiver. Also a user may not send money to himself.

`POST /v1/transactions/transfer`

__Request for admins__
```
{
   "sender": <id of the sender>,
   "receiver": <id of the receiver>,
   "amount": <amount of money transfered in cent as long>
}
```

__Request for users__
```
{
   "receiver": <id or name of the receiver>,
   "amount": <amount of money transfered in cent as long>
}
```

----
#### Make new deposit transaction

`POST /v1/transactions/deposit`

This will add money the the account of the currently logged in user.

__Request__

```
{
   "amount":<amount of money in cent as long>
}
```


----
#### Make new withdrawal transaction

`POST /v1/transactions/withdraw`

This can only be done by admins.

Andmins can only take money from themselves, if they want to take money from other users they first have to transferee
it to themselves.

__Request__

```
{
   "amount":<amount of money in cent as long>
}
```


----
#### Make new [order](https://www.youtube.com/watch?v=BJcpajX7EdU) transaction

`POST /v1/transactions/order`

This can only be done by admins.

__Request__

```
{
   "amount":<amount of money the order cost in cent as long>,
   "orders": [
      {
         "product_info": <product info id>,
         "amount": <count of this product ordered>
      },
      <more products added by this order>
   ]
}
```


----
#### Make new purchase transaction

`POST /v1/transactions/purchases`

__Request__

```
{
   "orders": [
      {
         "product": <product id>,
         "amount": <count of this product ordered>
      },
      <more products added by this order>
   ]
}
```

----
# Authentication
The authentication on Matohmat is done through [Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication).
However it depends if you want to make a Admin login or just a regular user login.

The basic auth header for an admin is simply
`Authorization: Basic base64(<admin_name>:<admin_password>)`.
So as an example the user `test` and the password `passwd` would generate this header:
`Authorization: Basic dGVzdDpwYXNzd2Q=`.

The auth header for a regular user is the base64 encoded double dot followed by the sha1 hash of the id entered by the user (mostlikely the RFID of the Ohmcard):
`Authorization: Basic base64(:sha1hash(<user_id>))`
So if a user has the "id": `123456789` this will undergo these steps:

1. `sha1sum(123456789)` =  `98O8HYCOBHMq32eZZczDTKeuNEE=`
3. Build auth string: `98O8HYCOBHMq32eZZczDTKeuNEE=:`
4. `base64(98O8HYCOBHMq32eZZczDTKeuNEE=:)` = `OThPOEhZQ09CSE1xMzJlWlpjekRUS2V1TkVFPToK`
5. Build auth header: `Authorization: Basic OThPOEhZQ09CSE1xMzJlWlpjekRUS2V1TkVFPToK`

----

# Error handlying

First of all a HTTP code will be returned by every request. This is the codes used by this api, and what they mean:

Success codes:

- `200` when send a query and the answer was sent
- `201` when a new entry was created through a POST
- `202` when a change was successfully done through a PATCH
- `200` when an entry was successfully removed through a DELETE

Failure codes:

- `400` the request was malformed
- `401` when something was requested but the login was wrong
- `404` If an entry is not found after a query was send.

Error codes:

- `500` Server errored
