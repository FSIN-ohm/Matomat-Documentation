# MatΩhmat documentation

MatΩhmat is a cash system designed to cell Mate, sweets and other joyful goods :D.
MatΩhmat is created by and for the [students of the IT-Fculty](https://fachschaft.in.th-nuernberg.de/) of
the [TH Nuermberg](https://th-nuernberg.de) University. 
It is the successor of the [k4cg](https://k4cg.org) [Matomat](https://github.com/k4cg/matomat) and
the [Fablabl NBG](https://fablab-nuernberg.de/) [Matomat](https://matom.at/).

MatΩhmat allows users to register and have an account where they can pay money to. From this account they can then
buy what is Provided by the [Fachschaft](https://fachschaft.in.th-nuernberg.de/).

The design of the system is modern, open, and mostly designed with technologies that IT students of
[TH Nuermberg](https://th-nuernberg.de) are able to understand. This should enable future generations of students
to administrate, and extend the system with cool features.

MatΩhmat consists of three major parts, 

 - a Server using MySQL to store all Persistent information, and handle Background Jobs.
 - a web based Admin frontend with which is possible to administrate the system.
 - a frontend based on a Raspbery PI which handles the interaction with the users.

![Overview](img/matohmat_arch.svg)

### The Server
The core is the Server with which all other parts of MatΩhmat communicate with through a [REST-API](https://en.wikipedia.org/wiki/Representational_state_transfer)
 based on the [JSON](https://de.wikipedia.org/wiki/JavaScript_Object_Notation) protocol.
The Server is written in java using the [Spring Framework](https://spring.io/) for providing the REST-API, and
[Hibernate](http://hibernate.org/orm/documentation/5.3/) to communicate with the [MySQL](https://www.mysql.com/de/) database.

### The Admin-Fronted
The Admin-Frontend is written HTML, CSS, and [TypeScript](https://www.typescriptlang.org/) using the [Angular](https://angular.io/)
framework. It is a web application that can be delivered by a static web sever like [Nginx](https://www.nginx.com/resources/wiki/).
All the the persistent data it need it gets form the [MatΩhmat Server](#the_server).

### The User-Frontend
The User-Frontend is a little wooden box containing a touch display a RaspberryPI, and a RFID scanner. It displays a
touch friendly webpage hosted on the device itself. The page comunicates directly with the [MatΩhmat Server](#the_server)
through its REST-API. It is created using pure HTML5, CSS and JavaScript.

