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

- `items=<number>` number of items per page. If this parameter is not given it will return all admins in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.


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

- `items=<number>` number of items per page. If this parameter is not given it will return all users in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.

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
   "client_key":"09d374ebf788ca96d5c5ad8cfc778317d8efa692766accc609eb5932ccf19c94",
   "auth_hash":"HansPeter"
}
```

----
#### Delete an user

`DELETE /v1/users/<admin id>`

This request requires Admin privileges.

----

# Product

__Endpoint: `/products`__

#### Query all products

`GET /v1/products`

- `items=<number>` number of items per page. If this parameter is not given it will return all products in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `onlyAvaialbe=<true/false>` if true will only return products that are available. If false or not given it will return all products in database.

__Reply__
```
[
   {
       "id":<product id as long>,
       "name":<product name as string>,
       "price":<product price in cent as ing>,
       "thumbnail":<url of image thumbnail as string>,
       "reorder_point":<reorder point as int>,
       "barcode":<barcode of the product as string>,
       "is_available":<is available as boolean>,
       "items_per_crate":<items or bottles per crate>
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
    "name":<product name as string>,
    "price":<product price in cent as ing>,
    "thumbnail":<url of image thumbnail as string>,
    "reorder_point":<reorder point as int>,
    "barcode":<barcode of the product as string>,
    "is_available":<is available as boolean>,
    "items_per_crate":<items or bottles per crate>
}
```
----
#### Add product

`POST /v1/products`

This request requires Admin privileges.

__Request__

```
{
   "name":<product name as string>,
   "price":<product price in cent as long>,
   "thumbnail":<url of image thumbnail as string>,
   "reorder_point":<reorder point as int>,
   "barcode":<barcode of the product as string>,
   "items_per_crate":<items or bottles per crate>
}   
```

----
#### Update product

`PATCH /v1/products/<product id>`

This request requires Admin privileges.

This is ment for changing the values of already existing products.

__Request__

```
{
   "name":<product name as string>,
   "price":<product price in cent as long>,
   "thumbnail":<url of image thumbnail as string>,
   "reorder_point":<reorder point as int>,
   "barcode":<barcode of the product as string>,
   "items_per_crate":<items or bottles per crate>
}   
```

----
#### Delete a product

`DELETE /v1/products/<product id>`

This request requires Admin privileges.

----
# Transaction

__Endpoint: `/transactions`__

#### Query transactions

`GET /v1/transactions`

If this request is made by a regular user it will only display the transactions of the current user.

If a transaction is of type `order` or `purchase`it will also contain an attribute `products` which will be a list
of products that where ordered or soled.

An order is a special kind of purchase, where there is not only an `amount`, by means the value of the sum of the price
of all products, but also a `buy_cost`, which is the price we paied at the marched for the products.
So the `amount` will be the price for which we sell the products, and `buy_cost` will be the price we buy them with.
The difference between `buy_cost` and `amount` will then be our __income__.

- `items=<number>` number of items per page. If this parameter is not given it will return 100 items of the first page.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `user=<user_id>` get all transactions of the given user. __Atention__ this requires admin privileges. made.
- `type=<purchse/deposit/withdraw/transfer/order/all>` this will only show the transactions of a specific type. If left out or set to `all` it will show all the

__Reply__
```
[
    {
        "id":<transaction id as long>,
        "date":<transaction date as string>,
        "sender":<user id of the sender as long>,
        "receiver":<user id of the receiver as long>,
        "amount":<amount of money transfered in cent as long>,
        "transaction_type":"<type of the transaction as string>
    },
    {
        "id":<transaction id as long>,
        "date":<transaction date as string>,
        "sender":<user id of the sender as long>,
        "receiver":<user id of the receiver as long>,
        "amount":<amount of money transfered in cent as long>,
        "transaction_type":"purchase"
        "products": [
          <product id>,
          <produtc id>,
          <etc>
       ]
    },
    {
        "id":<transaction id as long>,
        "date":<transaction date as string>,
        "sender":<user id of the sender as long>,
        "receiver":<user id of the receiver as long>,
        "amount":<amount of money transfered in cent as long>,
        "buy_cost":<amount of money the order cost at the merchand in cent as long>,
        "transaction_type":"order",
        "products": [
            <product id>,
            <produtc id>,
            <etc>
        ]
    },
    <more transactions>
]
```
----
#### Query one specific transaction

__CAUTION__ this may only return transactions of the current user (if not admin). Return a 404 if the transaction
exists but does not belong the the user currently logged

`GET /v1/transactions/<transaction id>`

__Reply__
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

`POST /transactions/withdrawl`

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
   "buy_cost":<amount of money the order cost in cent as long>,
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

The auth header for a regular user is the double dot followed by the first 25 signes of the base64 encoded sha2 hash of the id entered by the user (mostlikely the RFID of the Ohmcard):
`Authorization: Basic base64(:head(count=25, sha256sum(format=base64, <user_id>)))`
So if a user has the "id": `123456789` this will undergo these steps:

1. `sha256sum(123456789)` =  `6d78392a5886177fe5b86e585a0b695a2bcd01a05504b3c4e38bc8eeb21e8326`
2. `head(count=25, 6d78392a5886177fe5b86e585a0b695a2bcd01a05504b3c4e38bc8eeb21e8326` = `6d78392a5886177fe5b86e585`
3. Build auth string: `:6d78392a5886177fe5b86e585`
4. `base64(:6d78392a5886177fe5b86e585)` = `OjZkNzgzOTJhNTg4NjE3N2ZlNWI4NmU1ODU=`
5. Build auth header: `Authorization: Basic OjZkNzgzOTJhNTg4NjE3N2ZlNWI4NmU1ODU=`

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

All the error codes beside the success code 200 will also return a JSON containing an error message which can be displayed by the client:

```
{
   "error_message":"<this is an error message>"
}
```
