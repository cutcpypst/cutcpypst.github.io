---
layout: post
title:  "How to use cookiecutter-django template"
date:   2016-06-19 23:27:49 +0530
categories: 
---

Have you ever wanted to automate all these boring things that you have to do while setting up new django project? Like writing proper settings, setting up whole folder structure, adding docs, readmes, setup.py etc? There is app that can do these mundane actions for you: Cookiecutter Django.

Q: What you get after reading this post?

A: Basic understanding how you can automate boring tasks and get exciting ones done!

Let's start with cookiecutter. What exactly is it? It's CLI tool for automatically generating projects from given template. What do I mean by that?

Suppose that when you make new python project you want certain things to be setup the same way every time. For instance: setup.py or docs folders. Thanks to cookiecutter it's possible to automate these things so every time you make the new project the boring stuff is already made for you.

Django Cookiecutter is made especially for django applications. Based on GitHub README it provides:

For Django 1.9
Renders Django projects with 100% test coverage
Twitter Bootstrap v4.0.0 - alpha
End-to-end via Hitch
AngularJS
12-Factor based settings via django-environ
Optimized development and production settings
Registration via django-allauth
Comes with custom user model ready to go.
Grunt build for compass and livereload
Basic e-mail configurations for sending emails via Mailgun
Media storage using Amazon S3
Docker support using docker-compose for development and production
Procfile for deploying to Heroku
And some additional features:

Serve static files from Amazon S3 or Whitenoise
Configuration for Celery
Integration with MailHog for local email testing
Integration with Sentry for error logging
Integration with NewRelic for performance monitoring
Integration with Opbeat for performance monitoring
So it can make a huge improvement in initial project configuration. Let's try this out!

After installation via pip by:

$ pip install cookiecutter
Then after typing:

$ cookiecutter https://github.com/pydanny/cookiecutter-django.git
And answering all these questions:

project_name [project_name]: blog_cookiecutter
repo_name [blog_cookiecutter]: blog-cookiecutter
author_name [Your Name]: Krzysztof Żuraw
email [Your email]: kz@example.com
description [A short description of the project.]: This is example of cookiecutter django usage
domain_name [example.com]: example.com
version [0.1.0]: 0.0.1
timezone [UTC]: UTC
now [2016/01/29]: 2016/02/18
year [2016]:
use_whitenoise [y]: n
use_celery [n]: y
use_mailhog [n]: n
use_sentry [n]: y
use_newrelic [n]: n
use_opbeat [n]: n
windows [n]: n
use_python2 [n]: n
Select open_source_license:
1 - MIT
2 - BSD
3 - Not open source
Choose from 1, 2, 3 [1]: 1
You got the simple django application with celery, sentry, tests, grunt, less and docker.

Folder structure looks as follows:

$ tree . -a -L 3
.
└── blog-cookiecutter
    ├── app.json
    ├── blog-cookiecutter
    │   ├── contrib
    │   ├── __init__.py
    │   ├── static
    │   ├── taskapp
    │   ├── templates
    │   └── users
    ├── compose
    │   ├── django
    │   └── nginx
    ├── config
    │   ├── __init__.py
    │   ├── settings
    │   ├── urls.py
    │   └── wsgi.py
    ├── CONTRIBUTORS.txt
    ├── .coveragerc
    ├── dev.yml
    ├── docker-compose.yml
    ├── Dockerfile
    ├── Dockerfile-dev
    ├── .dockerignore
    ├── docs
    │   ├── conf.py
    │   ├── deploy.rst
    │   ├── docker_ec2.rst
    │   ├── index.rst
    │   ├── __init__.py
    │   ├── install.rst
    │   ├── make.bat
    │   └── Makefile
    ├── .editorconfig
    ├── env.example
    ├── .gitattributes
    ├── .gitignore
    ├── Gruntfile.js
    ├── install_os_dependencies.sh
    ├── install_python_dependencies.sh
    ├── LICENSE
    ├── manage.py
    ├── package.json
    ├── Procfile
    ├── .pylintrc
    ├── README.rst
    ├── requirements
    │   ├── base.txt
    │   ├── local.txt
    │   ├── production.txt
    │   └── test.txt
    ├── requirements.apt
    ├── requirements.txt
    ├── runtime.txt
    ├── setup.cfg
    ├── tests
    │   ├── all.settings
    │   ├── base.yml
    │   ├── ci.settings
    │   ├── engine.py
    │   ├── hitchreqs.txt
    │   ├── register-and-log-in.test
    │   ├── system.packages
    │   └── tdd.settings
    └── .travis.yml
So as this application follows the 12-Factor application guidelines most of django settings variables are set in an environment so it's good to run this in docker.

To install docker on ubuntu type:

$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates
$ sudo sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
Now it's time to add entry in /etc/apt/sources.list.d/docker.list for Ubuntu 14.04

deb https://apt.dockerproject.org/repo ubuntu-trusty main
And run:

$ sudo apt-get update
$ sudo apt-get install linux-image-extra-$(uname -r)
$ sudo apt-get install docker-engine
Then verify if everything is installed:

$ sudo service docker start $ sudo docker run hello-world
Now you have Docker! But to be able to use docker in cookiecutter-django project there is need for installing additional package called docker-compose. It allows to run multiple docker containers.

Installation is simple:

$ sudo pip install docker-compose
$ docker-compose --version
docker-compose version 1.6.0, build d99cad6
So it's time to fire up the installation:

$ sudo docker-compose -f dev.yml build
$ sudo docker-compose -f dev.yml up -d
-f flag means that we specify which yml file is taken for configuration (by default it is docker-compose.yml) and -d flag in the second command is for detached mode.

Now it's time to run basic django commands to migrate data to database and to create superuser:

$ sudo docker-compose -f dev.yml run django python manage.py makemigrations
$ sudo docker-compose -f dev.yml run django python manage.py migrate
$ sudo docker-compose -f dev.yml run django python manage.py createsuperuser
Then go to the localhost:8000 and you can see that Django application works!:


