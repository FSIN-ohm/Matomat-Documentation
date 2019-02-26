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

- `items=<number>` number of items per page. If this parameter is not given it will return all users in one request.
- `page=<number>` only works in combination with the `items` parameter. If not given, the first page will be returned.

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
