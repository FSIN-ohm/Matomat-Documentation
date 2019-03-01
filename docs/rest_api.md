REST API
========

All parts of Matohmat communicate through a common JSON based REST API.
Here is the documentation of it, so one who wants to program a client, or change the server.

----
# Admin

- Endpoint: `/admin`
- Endpoint: `/admins`

#### Query all admins `GET /admins`

- `items=<number>` number of items per page. If this parameter is not given it will return all admins in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.

----

# User

- Endpoint: `/user`
- Endpoint. `/users`

#### Query all users `GET /users`
This request requires Admin privileges.

- `items=<number>` number of items per page. If this parameter is not given it will return all users in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.

#### Query one specific user `GET /user/<user_id>`
This request requires Admin privileges.



----

# Product

- Endpoint: `/product`
- Endpoint: `/products`

#### Query all products `GET /products`

- `items=<number>` number of items per page. If this parameter is not given it will return all products in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.

----

# Transaction

- Endpoint: `/transaction`
- Endpoint: `/transactions`

#### Query transactions `GET /transactions`

- `items=<number>` number of items per page. If this parameter is not given it will return 100 items of the first page.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.
- `user=<user_hash>` get all transactions of the given user.

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
