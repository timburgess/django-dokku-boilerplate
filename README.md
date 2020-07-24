# django-dokku-boilerplate

A simple boilerplate for Django using Dokku

Dokku provides a fantastic git-oriented way of deploying applications.
This is a minimum boilerplate (no Postgres, no S3, no kitchen sink) to get you started with Dokku so you can focus
on learning Dokku rather than hacking around in Django internals

In a nutshell,

- setup an Ubuntu 18.04.03 LTS machine running Dokku version 0.21.3

- create an app name: `dokku apps:create simple-dokku`

- set your appenvironment:

```
dokku config:set simple-dokku DJANGO_ALLOWED_HOSTS='*'
dokku config:set simple-dokku DJANGO_SETTINGS_MODULE='config.settings.local'
dokku config:set simple-dokku DJANGO_SECRET_KEY='your-custom-secret-key-here'
```

- clone this repo

- setup dokku as a remote repository `git remote add dokku dokku@my-ubuntu-box-here:simple-dokku`

- push to dokku! `git push dokku master`

