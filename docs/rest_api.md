REST API
========

All parts of Matohmat communicate through a common JSON based REST API.
Here is the documentation of it, so one who wants to program a client, or change the server.

----
# Admin

__Endpoint: `/admins`__

#### Query all admins
`GET /admins`

This request requires Admin privileges.

- `items=<number>` number of items per page. If this parameter is not given it will return all admins in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.


__Reply__

```json
{
   data:[
      {
         id:<admin id as long>,
         user_id:<user id as long>,
         balance:<balance in cent as integer>,
         last_seen:<last login date as string>,
         available:<is user available as boolean>,
         username:<admin username>,
         email:<email address>
      },
      <furhter admins>
   ],
   status: "ok"
}
```

#### Query one specific admin
`GET /admins/<admin_id>`

This request requires Admin privileges.

__Reply__
```
{
   data:{
      id:<admin id as long>,
      user_id:<user id as long>,
      balance:<balance in cent as integer>,
      last_seen:<last login date as string>,
      available:<is user available as boolean>,
      user_name:<admin username>,
      email:<email address>
   },
   status: "ok"
}
```

#### Query one specific admin
`GET /admins/me`

This request requires Admin privileges.

It will return the user represented by the basic auth credentials.

__Reply__
```
{
   data: {
      id:<admin id as long>,
      user_id:<user id as long>,
      balance:<balance in cent as integer>,
      last_seen:<last login date as string>,
      available:<is user available as boolean>,
      user_name:<admin username>,
      email:<email address>
   },
   status: "ok"
}
```

#### Create a new admin

This request requires Admin privileges.

Every existing admin can create new admins.

`POST /admins`

__Request__

```
{
   user_name:<admin username>,
   password:<admin password>,
   email:<email address>
}
```

__Reply__
```
{
   status:"ok"
}
```

----

# User

__Endpoint: `/users`__

#### Query all users

`GET /users`

This request requires Admin privileges.

- `items=<number>` number of items per page. If this parameter is not given it will return all users in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.

__Reply__

```
{
   data:[
      {
         id:<user id as long>,
         balance:<balance in cent as integer>,
         last_seen:<last login date as string>,
         available:<is user available as boolean>
      },
      <furhter users>
   ],
   status:"ok"
}
```

#### Query one specific user

`GET /users/<user_id>`

This request requires Admin privileges.

__Reply__

```
{
   data: {
      id:<user id as long>,
      balance:<balance in cent as integer>,
      last_seen:<last login date as string>,
      available:<is user available as boolean>
   },
   status: "ok"
}
```


#### Query a user based on his basic auth header

`GET /users/me`

This will require a user to send a basic auth header with his credentials.
It will return the user represented by the basic auth credentials.

__Reply__

```
{
   data: {
      id:<user id as long>,
      balance:<balance in cent as integer>,
      last_seen:<last login date as string>,
      available:<is user available as boolean>
   },
   status: "ok"
}
```


#### Create new user

`POST /user`

A create new user may be sent without auth header, however if no basic auth header is given it requires
that the request contains a valid client key. These keys are used to permit privileged clients to create new users.
If an admin auth header is given the `client_key` can be empty or left out, and should not even be used here.
Regular users are not permitted to register new users. Only admins or priviliged clients.
The body of the post must contain this data:
```
{
   client_key:<key of the client priviliged to create new users. Can be left out if adbmin basic auth is given>,
   auth_hash:<first 25 bytes of the auth hash (sha256) generated from the user id>
}
```

Example:
```
{
   client_key:"09d374ebf788ca96d5c5ad8cfc778317d8efa692766accc609eb5932ccf19c94",
   auth_hash:"6d78392a5886177fe5b86e585"
}
```

__Reply__

```
{
   status: "ok"
}
```


----

# Product

__Endpoint: `/products`__

#### Query all products

`GET /products`

- `items=<number>` number of items per page. If this parameter is not given it will return all products in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `onlyAvaialbe=<true/false>` if true will only return products that are available. If false or not given it will return all products in database.

__Reply__
```
{
   data:[
      {
         id:<product id as long>,
         name:<product name as string>,
         price:<product price in cent as ing>,
         thumbnail:<url of image thumbnail as string>,
         reorder_point:<reorder point as int>,
         is_available:<is available as boolean>
      },
      <more products>
   ],
   status:"ok"
]
```

#### Query for specific product

`GET /products/<product_id>`

__Reply__
```
{
   data: {
      id:<product id as long>,
      name:<product name as string>,
      price:<product price in cent as ing>,
      thumbnail:<url of image thumbnail as string>,
      reorder_point:<reorder point as int>,
      is_available:<is available as boolean>
   },
   status:"ok"
}
```

#### Add product

`POST /products`

This request requires Admin privileges.

__Request__

```
{
   name:<product name as string>,
   price:<product price in cent as long>,
   thumbnail:<url of image thumbnail as string>,
   reorder_point:<reorder point as int>
}   
```

__Reply__
```
{
   status:"ok"
}
```

#### Update product

`UPDATE /products/<product id>`

This request requires Admin privileges.

This is ment for changing the values of already existing products.

__Request__

```
{
   name:<product name as string>,
   price:<product price in cent as long>,
   thumbnail:<url of image thumbnail as string>,
   reorder_point:<reorder point as int>
}   
```

__Reply__
```
{
   status:"ok"
}
```

----

# Transaction

__Endpoint: `/transactions`__
__Endpoint: `/transfares`__
__Endpoint: `/purchases`__

#### Query transactions

`GET /transactions`

If this request is made by a regular user it will only display the transactions of the current user.

- `items=<number>` number of items per page. If this parameter is not given it will return 100 items of the first page.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `user=<user_hash>` get all transactions of the given user. __Atention__ this requires admin privileges. made.
- `type=<purchse/deposit/withdraw/transfer/order/all>` this will only show the transactions of a specific type. If left out or set to `all` it will show all the

__Reply__
```
{
   data:[
      {
         id:<transaction id as long>,
         date:<transaction date as string>,
         sender:<user id of the sender as long>,
         recipient:<user id of the recipient as long>,
         amount:<amount of money transfared in cent as long>,
         transaction_type:<type of the transaction as string>
      },
      <more products>
   ],
   status:"ok"
]
```

#### Query one specific transaction

__CAUTION__ this may only return transactions of the current user (if not admin). Return a 404 if the transaction
exists but does not belong the the user currently logged

`GET /transactions/<transaction id>`

__Reply__
```
{
   data: {
      id:<transaction id as long>,
      date:<transaction date as string>,
      sender:<user id of the sender as long>,
      recipient:<user id of the recipient as long>,
      amount:<amount of money transfared in cent as long>,
      transaction_type:<type of the transaction as string>
   },
   status:"ok"
]
```

#### Query purchases

Purchases have more data embedded then regular transactions therefore there is an extra purchases endpoint.
If this request is made by a regular user it will only display the purchases of the current user.


- `items=<number>` number of items per page. If this parameter is not given it will return 100 items of the first page.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `user=<user_hash>` get all transactions of the given user. __Atention__ this requires admin privileges. made.


__Reply__
```
{
   data:[
      {
         id:<transaction id as long>,
         date:<transaction date as string>,
         sender:<user id of the sender as long>,
         recipient:<user id of the recipient as long>,
         amount:<amount of money transfared in cent as long>,
         products: [
            <product id as long>,
            <product id as long>,
            <etc>
         ]
      },
      <more products>
   ],
   status:"ok"
]
```

#### Query one specific purchases

__CAUTION__ this may only return purchases of the current user (if not admin). Return a 404 if the transaction
exists but does not belong the the user currently logged

`GET /transactions/<purchase id>`

__Reply__
```
{
   data: {
      id:<transaction id as long>,
      date:<transaction date as string>,
      sender:<user id of the sender as long>,
      recipient:<user id of the recipient as long>,
      amount:<amount of money transfared in cent as long>,
      products: [
         <product id as long>,
         <product id as long>,
         <etc>
      ]
   },
   status:"ok"
]
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
So if a user has the id: `123456789` this will undergo these steps:

1. `sha256sum(123456789)` =  `6d78392a5886177fe5b86e585a0b695a2bcd01a05504b3c4e38bc8eeb21e8326`
2. `head(count=25, 6d78392a5886177fe5b86e585a0b695a2bcd01a05504b3c4e38bc8eeb21e8326` = `6d78392a5886177fe5b86e585`
3. Build auth string: `:6d78392a5886177fe5b86e585`
4. `base64(:6d78392a5886177fe5b86e585)` = `OjZkNzgzOTJhNTg4NjE3N2ZlNWI4NmU1ODU=`
5. Build auth header: `Authorization: Basic OjZkNzgzOTJhNTg4NjE3N2ZlNWI4NmU1ODU=`

----

# Error handling
If something fails on the server site you will get a JSON reply containing an error. This applies for querys as well
as commands equally. Every reply form the server will have a `status` attribute in the JSON reply. This attribute can
either be `ok` or `failed`. If something failed there will also be an `error_message` attribute containing a string
that gives further description about why the operation failed.
This is an example of what it will lock like if an operation failed:

```
{
   status: "failed",
   error_message: "Could not connect to database"
}
```

__ATENTION__ If you process a reply from a server make sure to always process the `status` reply first, as command/query specific
attributes are not send if the operation failed.
