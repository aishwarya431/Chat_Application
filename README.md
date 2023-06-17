# ChatApp  #

![](http://g.recordit.co/JYruQDLd0h.gif)

A small functional person-to-person message center application built using Django.
It has a REST API and uses WebSockets to notify clients of new messages and 
avoid polling.

## Architecture ##
 - When a user logs in, the frontend downloads the user list and opens a
   Websocket connection to the server (notifications channel).
 - When a user selects another user to chat, the frontend downloads the latest
   15 messages (see settings) they've exchanged.
 - When a user sends a message, the frontend sends a POST to the REST API, then
   Django saves the message and notifies the users involved using the Websocket
   connection (sends the new message ID).
 - When the frontend receives a new message notification (with the message ID),
   it performs a GET query to the API to download the received message.



### Database ###
For this demo, I'm using a simple MySQL setup. If more performance is required, 
a MySQL cluster / shard could be deployed.

PD: I'm using indexes to improve performance.


## Run ##

0. move to project root folder


1. Create and activate a virtualenv (Python 3)
```bash
pipenv --python 3 shell
```
2. Install requirements
```bash
pipenv install
```
3. Create a MySQL database
```sql
CREATE DATABASE chat CHARACTER SET utf8;
```
4. Start Redis Server
```bash
redis-server
```

5. Init database
```bash
./manage.py migrate
```
6. Run tests
```bash
./manage.py test
```

7. Create admin user
```bash
./manage.py createsuperuser
```

8. Run development server
```bash
./manage.py runserver
```

To override default settings, create a local_settings.py file in the chat folder.

Message prefetch config (load last n messages):
```python
MESSAGES_TO_LOAD = 15
```
