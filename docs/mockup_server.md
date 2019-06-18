Mockup Server
=============

The mockup server is used for developing the [REST](/Matomat-Documentation/rest_api/) request of the clients without having to utilize the actual server.
This is used for the time the actual server does not have full functionality yet, or when the full server is not needed.

The mockup server has no logic embodied, on POST, DELETE and PATCH methods it will always respond with a success
even if the body of the request was malformed or wrong.
For GET however it will return static JSON files which have been written for test cases.

### Setup

The Mockup Server itself is written in Python using [flask](http://flask.pocoo.org/). For setting up Python on your OS please see the according
section of the [Documentation](/documentation/#installation) which describes how to use Python for mkdocs.

When you installed python you may want to pull the Matohmat [server repository](https://github.com/FSIN-ohm/Matomat-Server).
Here enter the [mockup](https://github.com/FSIN-ohm/Matomat-Server/tree/HEAD/mockup) directory. You will find the `mockup_server.py` file
in here. Execute this file with Python.

### Usage

You can use [Postman](https://www.getpostman.com/) to do test requests against the server (this works for mockup server as well as the actuall
server).
