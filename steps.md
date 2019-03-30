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

Initialize the DB file using the flask command

`flask init-db`
