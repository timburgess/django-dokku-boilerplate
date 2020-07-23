# django-dokku-boilerplate

A simple boilerplate for Django using Dokku

Dokku provides a fantatsic git-oriented way of deploying applications.
This is a minimum boilerplate (no Postgres, no S3, no kitchen sink) to get you started with Dokku so you can focus
on learning Dokku rather than hacking around in Django internals

To run locally:

export DJANGO_SETTINGS_MODULE=config.settings.local

python manage.py migrate

python manage.py showmigrations

python manage.py runserver

