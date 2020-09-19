# Working with Docker

* docker-compose.yml

* sudo docker-compose up

## Running in production

```Dockerfile
FROM ruby:2.2.2
RUN bundle config ‐‐global frozen 1
RUN mkdir ‐p /usr/src/app
WORKDIR /usr/src/app
ONBUILD COPY Gemfile /usr/src/app/
ONBUILD COPY Gemfile.lock /usr/src/app/
ONBUILD RUN bundle install
ONBUILD COPY . /usr/src/app
```
> The instructions that show ONBUILD will not be executed when the image is generated, all will be postponed. These triggers will be fired only when that image is inherited in another Dockerfile. It is approach is ideal for creating stand-alone images for a project.

> Still thinking about the production environment, we don't need to change the existing recipe in docker-compose.yml, as docker-compose allows the definition of a separate file with the settings appropriate for the production environment, in this case, production.yml.

