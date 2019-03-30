# Official Flask tutorial
Setting up a simple blog called flaskr

## Project Layout
typical layout:
* flaskr/ - the python package contaiing your app code and files
* tests/
* venv/ a python venve
* installation files
* version control (git)
* other project files

setup gitignore with \*.pyc, __pycache__/, instance/, .pytest_cache/, .coverage, htmlcov/ dist/ build/ \*.egg-info/

## app setup
everything in the all will be registered with the `Flask` class

You can create a global Flask instance directly at the top of your code. But this gets tricky as the project grows.

instead, create it inside a function, an application factory.

create the flaskr folder and init file. Add the application factory to it

try running the app

`export FLASK_APP=flaskr`
`export FLASK_ENV=development`
flask run

## define and access DB
we will use SQLite

setup db.py

`g` is a special object unique for each request. it stores data that might be accessed by multiple functions during the request

`current_app` is also a special object, points to the Flask app.

Create the tables in schema.sql, add the commands to run it to db.py:
* init_db, init_db_command, init_app
and in init add the code to register the functions

the `app.teardown_appcontext()` tells flask to call that function whe cleaning up after returning the reposing

`app.cli.add_command` adds a new command that can be called with `flask` command from command line

Initialize the DB file using the flask command (may need to run the export commands again)

`flask init-db`

## blueprints and views
A view function is the code you write to respond to requests

Flask matched patters to the incoming request to the view function that should handle it.

The view returns data that Flask turns into a response.

A blueprint is a way to organise related views. You regsiter the views with the blueprint, not the app. Then register the BP with the app.

We will create 2 BPs, one for authentication functions, the other for post functions. We'll put the code for each in a separate module.

write `auth.py`
import it into init

define the register view (HTML is in the next part)

What `register()` is doing:
e the decorator associstes the url with the view. When the user puts this address in the browser it will return this view
* If the user submits the form the request type will be POST and the code will start executing. If the user has just navigated to it the request type will be GET and the page will just render
* `request.form` is a dictionary mapping form keys
* `fetchone()` returns one row from the query
* after commiting the new user to the db the user is directed to the login page by `redirect`, where `url_for` takes a view (as a string) and return the URL associated with the view
* flash() just stores error messages. if the view took arguements they would be passed here.

write the login view
* `session` is a dict that stores access requests. When login is sucessful id is stored in a new session, stored in a cookie which is sent to the browser. the id stored on the session will be available for subsequent request.

add the function `load_logged_in_user()` to fetch the user data when handling requests

create logout function

create a login_required decorator

## Templates
store in templates/ in flaskr

Flask uses Jinja.

{{}} are expressions
{% %} are flow statements (if for)
blocks are denoted by start and end tags, not indentation.
g is automatically available in templates for useage (eg. g.user)

create a base.html file

special tags for base:
{% block title %} (match with {% extends 'base.html' %})
{% block header %}
{% block content %}

folder structure for templates should follow blueprints: create a folder for auth and register.html

input and password tags use required

set up login template

run the server and try registering a user
