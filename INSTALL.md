# Install/Deploy Guide

Instructions for deploying this boilerplate for Django with Dokku

Overview

Dokku provides a fantatsic git-oriented way of deploying applications. It is presumed that you have
some understanding of git, python and Django project setup.

This is a minimum boilerplate (no Postgres, no S3, no kitchen sink) to get you started with Dokku so you can focus
on learning/experimenting with Dokku rather than hacking around in Django internals.

There are essentially two parts to this:

1 - Setup a vanilla Dokku server & add an application environment to it for an app called 'simple-dokku'

2 - Clone this repo and deploy to the server



