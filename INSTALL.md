# Install/Deploy Guide

Instructions for deploying this boilerplate for Django with Dokku

# Overview

This repository is a minimum boilerplate (no Postgres, no S3, no kitchen sink) to get you started with Dokku so you can focus
on learning/experimenting with Dokku rather than hacking around in Django internals.

There are essentially two parts to get to a simple Django app running in Dokku:

1. Setup a vanilla Dokku server & add an application environment to it for an app called 'simple-dokku'

2. Clone this repo and deploy to the server

## Step 1 - Setup a Dokku server

1. Create a vanilla Ubuntu 18.04.03 LTS server in the cloud (Digital Ocean provides a Dokku 0.17.9 'droplet' but this repo will not deploy seamlessly on an older version of such as 0.17.9)

2. Create an ssh key pair if you don't have one already for password-less login. Your cloud provider should provide instructions about how to create a ssh public/private key pair.

3. Setup an ssh connection to your server with a `.ssh/config` file. Something like:
```
Host ubuntu1
        ForwardAgent yes
        Hostname 164.164.164.164
        Port 22
        ServerAliveInterval 60
        ServerAliveCountMax 60
```

4. Confirm that you can login to your server and have root or sudo access

5. Step through the commands to install [Dokku version 0.21.3](http://dokku.viewdocs.io/dokku/#install-apt). Some of the commands do a lot of work - expect it to take a few minutes.

6. Visit the server homepage and in the form, confirm the public key and hostname. Hostname can simply be the ip address. Do not check 'virtual naming'.

7. Create a new app: `dokku apps:create simple-dokku`

8. Create environment variables for the `simple-dokku` app. Set django allowed hosts with `dokku config:set simple-dokku DJANGO_ALLOWED_HOSTS='*'` As nginx front-ends django this can be *.

9. Add your secret key: `dokku config:set simple-dokku DJANGO_SECRET_KEY='your-secret-key here'

10. Tell Django where to find settings: `dokku config:set simple-dokku DJANGO_SETTINGS_MODULE='config.settings.local'`

11. Double-check your environment settings: `dokku config:show simple-dokku`


## Step 2 - Clone and deploy the application

1. Clone this repo: `git clone https://github.com/timburgess/django-dokku-boilerplate.git`

2. Add a remote repository (the dokku server): `git remote add dokku dokku@ubuntu1:simple-dokku`

3. Confirm the remote repository details: `git remote -v`

4. From the master branch: `git push dokku master` You should see something like the following:

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 476 bytes | 476.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0)
-----> Cleaning up...
-----> Building simple-dokku from herokuish...
-----> Adding BUILD_ENV to build environment...
-----> Python app detected
remote: cp: cannot stat '/tmp/build/requirements.txt': No such file or directory
       Skipping installation, as Pipfile.lock hasn't changed since last deploy.
-----> Installing SQLite3
-----> Discovering process types
       Procfile declares types -> release, web
-----> Releasing simple-dokku...
-----> Deploying simple-dokku...
 !     Release command declared: 'python manage.py migrate --noinput'
       Operations to perform:
         Apply all migrations: admin, auth, contenttypes, sessions
       Running migrations:
         Applying contenttypes.0001_initial... OK
         Applying auth.0001_initial... OK
         Applying admin.0001_initial... OK
         Applying admin.0002_logentry_remove_auto_add... OK
         Applying admin.0003_logentry_add_action_flag_choices... OK
         Applying contenttypes.0002_remove_content_type_name... OK
         Applying auth.0002_alter_permission_name_max_length... OK
         Applying auth.0003_alter_user_email_max_length... OK
         Applying auth.0004_alter_user_username_opts... OK
         Applying auth.0005_alter_user_last_login_null... OK
         Applying auth.0006_require_contenttypes_0002... OK
         Applying auth.0007_alter_validators_add_error_messages... OK
         Applying auth.0008_alter_user_username_max_length... OK
         Applying auth.0009_alter_user_last_name_max_length... OK
         Applying auth.0010_alter_group_name_max_length... OK
         Applying auth.0011_update_proxy_permissions... OK
         Applying sessions.0001_initial... OK
-----> App Procfile file found
       DOKKU_SCALE declares scale -> web=1
=====> Processing deployment checks
       No CHECKS file found. Simple container checks will be performed.
       For more efficient zero downtime deployments, create a CHECKS file. See http://dokku.viewdocs.io/dokku/deployment/zero-downtime-deploys/ for examples
-----> Attempting pre-flight checks (web.1)
       Waiting for 10 seconds ...
       Default container check successful!
-----> Running post-deploy
-----> App virtual host support disabled, skipping domains setup
-----> Creating http nginx.conf
       Reloading nginx
-----> Renaming containers
       Found previous container(s) (583b00ca3b0a) named simple-dokku.web.1
       Renaming container (583b00ca3b0a) simple-dokku.web.1 to simple-dokku.web.1.1595550273
       Renaming container (d030640443aa) fervent_raman to simple-dokku.web.1
-----> Shutting down old containers in 60 seconds
       583b00ca3b0a5712307f840239705e8224a66d40ce307745902f30dc58c33d2b
=====> Application deployed:
       http://ubuntu1:6632

To ubuntu1:simple-dokku
   f2f79fa..73b2133  master -> master
```
This repository uses `pipenv` (which Dokku understands) rather than a requirements.txt file. The requirements.txt error can be ignored.


5. The default Django web page should now be visible at your server on the deployment port advised
