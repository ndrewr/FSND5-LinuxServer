Udacity Fullstack Nanodegree - Project 5
=====================================================
### Linux Server Configuration - Ubuntu x Apache AWS server instance ###
### Author: Andrew Roy Chen, Sept 2015 ###



SUMMARY:
-----------------------------------------------------
Configured a base Ubuntu installation to serve FSND3's Catalog Flask application on an AWS virtual machine instance. HTTP server used is Apache 2 with the mod_wsgi module backed by a Postgresql database. 
The Catalog app can be visited at:
[http://ec2-52-11-99-253.us-west-2.compute.amazonaws.com/](http://ec2-52-11-99-253.us-west-2.compute.amazonaws.com/)

Note: the app uses GitHub authentication to authorize post creation and editing.



FILE STRUCTURE:
-----------------------------------------------------
This project packages the application files into the app folder 'yama'. Major modules and folders in the package include:
* views.py - includes app routes
* models.py
* forms.py
* static - static assets
* templates - html files supporting Jinja2

Alongside the app package are the flask configuration files, a module that sets up the default database and the app local development runner 'run.py'.



INSTRUCTIONS TO RUN:
-----------------------------------------------------
This project was first developed using 'Common code for the Relational Databases and Full Stack Fundamentals' courses found here:
http://github.com/udacity/fullstack-nanodegree-vm

The above describes a pre-configured vagrant setup. Quick start detailed here:
http://docs.vagrantup.com/v2/getting-started

Vagrant is **not** required to run the project.

Optional but recommended: setup a virtual environment for running the project. More details:
http://docs.python-guide.org/en/latest/dev/virtualenvs/

Project dependencies are listed under requirements.txt
Pip is used in the example below.
Whichever method, to run locally **ensure the neccessary python modules are already installed**:
```
pip install -r requirements.txt
```

Next to run on localhost:5100 (yes, 5100) simply:
```
python run.py
```

The above file will initialize a sample course database and start the app running locally on port 5100.
The test server uses Flask's built-in web server.

Users can view any course details but must login with Github credentials to access Create, Update and Delete privileges.



DEVELOPMENT NOTES:
-----------------------------------------------------
The project uses the Flask microframework and several key extensions to aid development. Data is stored in local test environment using SQLite but on deployment to Heroku PostgreSQL was used.

The Flask webserver is used to test locally but on deployment Gunicorn was used for better concurrent connection handling.

Flask configuration was a minor issue but to allow graders to run the app locally, private key information was included in the repo temporarily.


SUGGESTIONS:
-----------------------------------------------------
* allow only post creator to delete posts
* implement xml data endpoints
* CASCADE deletes (currently unneeded since no dependcy btwn db models...would change if Catalogs or Users could be deleted)
