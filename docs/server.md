Server
======

# Introduction

The matohmat server is REST Api server written in Java8 ans [spring](https://spring.io/). For the presistence it uses a [MySQL 8](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/) database. For hosting it it is highly suggested to use [Docker](https://www.Docker.com/) as there is already a Docker compose setup eavailable. Read more about the setup [here](#setup). The server is only supposed to provide the bare api, however the Docker setup will also serve the admin frontend and service to upload images to.

# Setup

For seting up the Matohmat server it is hightly suggested to use [Docker](https://www.Docker.com). It is possible to run it without, however you may have to configure a bit more in order to get it ruing without Docker. This or that way you may have to have experience in Docker in order to be able to read and understand the Docker config.

The Docker repository for the server can be found at the [matohmat-docker](https://github.com/FSIN-ohm/matohmat-Docker). The repo makes use of Docker and [docker-compose](https://docs.Docker.com/compose/). Before seting up the server make sure you understand both tools.

**What you need to know to setup a matohmat server**
- Basic understanding of **GIT**
- Good understanding of **docker**
- Good understanding of **nginx** and reverse proxy
- Basic understanding of **SQL**

#### Seting up the server
1. Make sure [Docker](https://docs.docker.com/install/) and [docker-compose](https://docs.docker.com/compose/install/) and [git](https://git-scm.com/) are installed on your system. These tools are both available for the moust major operation systems, including Linux, Windows and Mac.
2. Clone the matohmat-docker repository: `git clone https://github.com/FSIN-ohm/matohmat-docker.git`
3. Enter the matohmat-docker directory: `cd matohmat-docker`
4. Build the docker container by running: `docker-compose build`. This will take some while as many commands are run in order to build the image for the matohmat server.
5. When the build is done you can already test run the server with
   - `docker-compose up` for running in forground
   - `docker-compose up -d` for running the server in daemonized mode.
6. When you start the server for the first time the matohmat image might start faster then the database because database is not initialized yet. This will lead to a crash. If this happens simply wait for a bit and the restart the docker image: `docker-compose down` then `docker-compose up`.
7. In order to make your installation be ready for production you will want to [configure it](#configuration)

### Update
For Updating an already existing installation you will want to do these things
1. **BACKUP your installation directory.** Do an **offsite backup** at best.
2. Enter your docker installation directory
3. Shutdown the current installation: `docker-compose down`
4. Get the current docker configuration with `git update`
5. Build the update: `docker-compose build`
6. Start the new server again: `docker-compose up`

If something fails do a rollback, and if you can't get the update running [contact the developers](https://github.com/FSIN-ohm/matohmat-docker/issues/new).

### Configuration

Within the `matohmat-docker` directory you will find the `docker-compose.yml` file. *(Please make sure you understand [docker-compose version 2.1](https://docs.docker.com/compose/compose-file/compose-file-v2/))*

#### The db server
In the db server you will find the `volumes` entry  
- `${PWD}/data:/var/lib/mysql`  
This is pointing to the `data` direcotry. So here the database is saved. More about this [here](#storage).

#### The Server
**Ports:**  
by default the matohmat server is mapped to `127.0.0.1:8080`. You may want to change this line if you want to make the server run under another port. However be aware if you remove the `127.0.0.1` ip address the port will be exposed to the network, **even if you are running a firewall!**

**Environment:**

- `DB_HOST`: Host name of the database server. You may not need to edit this if you want to run pure docker setup, as it is already set to the host name of the database image `db`.
- `DB_SCHEMA`: The schema used for storing the matohmat tables into. You may not change this without editing the sql files inside the `database` directory.
- `DB_USER`: The user which accesses the database. You must not make this the root user! Also before changing the user here you might also edit the sql files inside the `database` direcotry.
- `DB_PWD`: Password for accsing the database as `DB_USER`. You may not change this without editing the sql files inside the `database` directory.
- `DEVICE_KEYS`: The file containing the device keys. You must edit this file. However editing this file will require you to rebuild the docker container!. Read more about this file at [device keys section](#device_keys).
- `ORIGIN`: Write down the **urls of the clients** that want to access the api server. If you don't list the URLs of the clients here you will get issues with [`CORS`](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) later, and you may not be able to send requests. Seperate the different urs with a `::`. Use the `null` as a url if you want to connect with a client that you load from a file and not from a server. However remove `null` in production.

### Storage

With in the matohmat-docker directory you will find the directory [data](https://github.com/FSIN-ohm/matohmat-docker/tree/master/data). Here the content of the database is saved.  
**!!!ATTENTION!!!** When backing up the matohmat data it is highly recomended to backup the whole `matohmat-docker` directory, not only the `data` folder.

### Device keys

The file `Server/device_keys.txt` contains a list of keys for clients that shall have the permission to add users. If a client is not listed here it is not able to add users. (Except the admin frontend as admins can always add new users).  
The keys listed in this file also have to be installed in the matohmat client.  
If you edit this file you will have to rebuild the docker container using `docker-compose build`

The scheme of the file looks like this: Evenry line contains a name of a key and the key itself. Name and key are seperated by a `:`. Like this:  
`<name of the key>:<the key it self in some giberish>`.  
**!!!ATTENTION!!!** There is already a default key. Remove this key and replace it by a custom one.

### Image hosting

In order to host images [Filebrowser](https://filebrowser.xyz/) is used. It is a simple tool hosted next to the server, which can be used to upload files to. Images droped into can have a public url which you can post into the product img url.
The data that filebrowser manages is Stored in `/filebrowser/data` and the database it uses for management is stored in `/filebrowser/db`.  
**!!!ATTENTION!!!** After the first setup you will want to change the admin password as by default the login will be admin:admin.

# Development setup

# Testing

# Releasing

Sadly role out/role out is not done automatically. You will have to build and deploy the server by hand.

This is what you will want to do to publish a new version.

**When something changed at the database**
Copy the `*.sql` file from the [/Database](https://github.com/FSIN-ohm/Matomat-Server/tree/master/Database) directory from the **matohmat-server** repo into the [/Database](https://github.com/FSIN-ohm/matohmat-docker/tree/master/Database) directory of the **matohmat-docker** repo.  
**!!ATTENTION!!** If you ever want to update the database scheme make sure you include an update routine. For already existing databases. 

**When something changes at the server**

1. **Run tests and make sure everything works before releasing.**
2. Edit the file `build.gradle` and update the version number:
   `version x.y`
3. Build the release version of the server:  
   Within the root directory of the Project run: `./gradlew assemble`
4. Enter the direcotry `/build/libs`. Here you will find find a `*.jar` file with the given version number in the name. This is the build server.
5. Create a new release in github:
    - Create a tag with the version number e.g.: `git tag v1.2 -m "v1.2"`
    - Push that tag to github e.g.: `git push origin v1.2`
    - Create a new [release draft](https://github.com/FSIN-ohm/Matomat-Server/releases/new)  
     ![new_draft](img/draft_new_version.png)
6. Attach the build build `*.jar` file to the release
     ![attatch_file](img/attach_file.png)
7. Hit `Publish Release`
8. Copy the link to the jar file
   ![copy_server_location](img/copy_server_location.png)
9. Replace [the line](https://github.com/FSIN-ohm/matohmat-docker/blob/2104f9bc36fd3fb1d7094f8053fbbb0c7fd672b9/Server/Dockerfile#L4) in the `Dockerfile` where the old server got downloaded with `wget`.
10. Make the docker setup be compatible with your new server release if necessary. Then push your changes on the matohmat-docker repo to master
11. **Test the matohmat-docker repo**
**!!ATTENTION!!** Always make sure the master branch of the matohmat-docker repository is **stable**.

# Architecture
