
# Chat Application

A simple real time chat application that covers Django authorization, web sockets and authentication.
## Assignment Problem Statement:

- Create a Django project, which include, user sign up, login (authentication). And a simple user dashboard which include entry to edit and update post login, first name, lase name and a profile picture (PostgreSQL database should be used).
- Second part of the same project is for you to create a simple one to one chat page, on local host, the page should contain text box, send button and a section where the message is printed.
- Third part of this project is for you to make this web socket secure. You don't have to add any certificates to make the protocol 'wss' instead of  'ws'. But you have to come up with a way to make sure just using the socket link no third party can see the streams. That mean you have to add a layer of security to view the stream.


## Features

- Login / signup in the application
- User authentication
- Update profile (ie name, email, picture)
- Create private chat rooms
- Create public chat rooms
- Authenticated WebSockets to prevent unauthorized access


## Technology / Library Used

**Client:** HTML, CSS, Jinja

**Server:** Python Django Framework, Channels, Websockets, Redis, PostgreSQL


## Installation of Required Packages

1. Install python
- https://www.python.org/downloads/

2. Clone or download the repository
```
git clone https://github.com/shubhamx939/ChatApp.git
cd ChatApp
```
3. Create and activate virtual environment
```
python -m venv dev

Linux or  Mac : source dev/bin/activate
Windows: dev\Scripts\activate.bat

```
3. Install required packages
```
python -m pip install --upgrade pip 
pip install -r requirements.txt
```

## Postgres Setup

Postgres needs to run as a service on your machine.

1. Download postgres : https://www.postgresql.org/download/

2. Run the installer file and go through the installation
    - Remember the superuser password you use. 
    - Port 5432 is the standard port for PostgreSQL

3. After installation confirm the service is running.
    - If it's not running then start it through application

4. Confirm you have access to database
    - Open Terminal
    - write ```psql postgres postgres```
    - means: "connect to the database named 'postgres' with the user 'postgres'". 'postgres' is the default root user name for the database.
    - use the password you have created while installing PostgreSQL

5. Setup Database for the Project
    - Write the following commands into PostgreSQL 
    - create a new database for our project
    ```
    CREATE DATABASE chatapp;
    ```
    - Create a new user that has permissions to use that database
    ```
    CREATE USER chatapp_admin WITH PASSWORD 'root@123';
    ```
    - Grant the new user all privileges on newly created DATABASE
    ```
    GRANT ALL PRIVILEGES ON DATABASE chatapp TO chatapp_admin;
    ```
    - Now the Database steup is done in PostgreSQL




## Run the Project

- Migrate to commit changes to database
```
python manage.py makemigrations
python manage.py migrate
```

- Create a superuser
```
python manage.py createsuperuser
```
- Run the server
```
python manage.py runserver
```





## Deployment on Server

To deploy this project on Server

- Run Redis in Docker
```
docker run -p 6379:6379 -d redis:5 
```

- Change the Channel layer to Redis Channel layer in settings.py

```
CHANNEL_LAYERS = {
    'default': {
        'BACKEND': 'channels_redis.core.RedisChannelLayer',
        'CONFIG': {
            "hosts": [('127.0.0.1', 6379)],
        },
    },
}
```


## Flow the ChatApp Project (After above steps once the server is up and running)

- User1 needs to register an account
- User1 needs to login with the same credentials
- User2 needs to register an account
- User2 needs to login with the same credentials
- After login user needs to enter the chat room name 
- User2 also needs to enter the same chat room to chat privately
- One to one chat only possible if both the users are into same chat room
- General or some other Public chat room is only created by admin credentials

## Steps to create a Public chat room
- Login to admin portal using admin credentials
- Create Room General and Random
- Now users can see the General and Random two public rooms to chat

## Different way you can authenticate the web socket in Django

- Session Authentication : If you authenticate your users, Django will create a session to track authenticated users and will assign a session variable to them. This session value will be sent to client-side via set-cookie header in response. And browser will save it, So all other requests will include this header by default. Even web-socket requests to django-channels.
- Token Authentication (provide token in HTTP header) : If the Authorization token is provided in header of HTTP requests, then you can create a custom Authentication Middleware, So that it intercepts requests coming with web-socket to django-channels routers.This Middleware, will check keys provided in HTTP header. If authorization is sent, will try to fetch it’s value. If the token name is Token then will try to find the corresponded Token Object for this in database. If so will fetch the user for that and update scope['user'] with that value.
- Token Authentication (No HTTP header required) : Whenever a socket is created between client-side and django-channels, Until the token is not provided in first request of user in socket’s data, and that token is not valid; No other data will be transferred between them.

## References

- https://docs.djangoproject.com/en/2.2/
- https://www.python.org/downloads/release/python-380/
- https://www.youtube.com/watch?v=UmljXZIypDc&list=PL-osiE80TeTtoQCKZ03TU5fNfx2UY6U4p&ab_channel=CoreySchafer
- https://realpython.com/getting-started-with-django-channels/
- https://channels.readthedocs.io/en/stable/
- https://www.youtube.com/watch?v=cw8-KFVXpTE&ab_channel=DennisIvy
- https://gist.github.com/rluts/22e05ed8f53f97bdd02eafdf38f3d60a