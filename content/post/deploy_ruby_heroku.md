+++
title = "How to deploy Ruby and Node app on Heroku using Docker - Part 1"
date = "2019-02-15"
draft = false
Categories = ["english", "devops", "docker", "ruby", "node"]
Tags = ["english", "devops", "docker", "ruby", "node"]
+++

## TL;DR
We needed to deploy a ruby+node application as a docker image on Heroku, but I didn't use Heroku cli to build it.  This document is about how we did the development, test, build, tag and deployment of a docker image on a Heroku application according to the best practices.

I will split this article into some parts. In this "Part 1" I will explain in details how I delivered the first version of a Dockerfile, without Multistage build,  to create the docker image and Docker compose file to bring up the whole development environment.

## Attention
If you copy and past some codes without reading the context it may not work properly on your environment.

I will present the code and tell the story behind this phase, and I will show how I improved this code after seeing the problem.

## Setup
To reproduce this article, you need to install these tools:

- Docker
- Docker compose
- Make

## The environments
We have three environments:

- Development: It is only on the developer's machine. 
- Staging: It is for QA analysis on Heroku app
- Production: It is the real app running on Heroku app

## Dockerization for developers first
I started this job providing docker environment to developers and getting feedbacks.

Here is my Dockerfile to developers:

```Dockerfile
FROM ruby:2.5.1 as builder

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - &&\
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - &&\
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update \
    && apt-get install -y locales \
                        graphviz \
                        imagemagick \
                        postgresql-client-9.6 \
                        yarn \
                        nodejs \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && /usr/sbin/locale-gen &&\
    rm -rf /var/lib/apt/lists/*

ENV GEM_HOME /gems/vendor
ENV GEM_SPEC_CACHE /gems/specs
ENV BUNDLE_APP_CONFIG /gems/vendor
ENV BUNDLE_PATH /gems/vendor
ENV BUNDLE_BIN /gems/vendor/bin
ENV PATH /app/bin:/gems/vendor/bin:$PATH
ARG RAILS_ENV
ENV RAILS_ENV=$RAILS_ENV

ENV APP_ROOT /app

WORKDIR $APP_ROOT/

RUN mkdir -p /gems

RUN groupadd -r app \
    && groupmod -g 1000 app \
    && useradd -g app -ms /bin/bash app \
    && chown app $APP_ROOT \ 
    && chown app /gems

USER app

COPY Gemfile.lock $APP_ROOT/
COPY Gemfile $APP_ROOT/
COPY package-lock.json $APP_ROOT/
COPY package.json $APP_ROOT/

RUN bundle install
RUN yarn install

EXPOSE 3000

CMD ["bundle","exec","rails","server","-b","0.0.0.0"]
```

This Dockerfile is responsible for setup everything that we needed to start the development job.

We didn't need to copy the code to docker context on the docker image creation phase. We mounted the source code inside the container on runtime:

```bash
docker build -t ruby_node_app:0.1 .
docker run -it --build-arg RAILS_ENV=development -v $PWD:/app ruby_node_app:0.1
```

The **RAILS_ENV** variable was used to change the gem installation behavior — some gems we didn't need to install in production.

### Dockefile, explained

We used this first section to set up the basis of this dev environment:

```Dockerfile
FROM ruby:2.5.1 as builder

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - &&\
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - &&\
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update \
    && apt-get install -y locales \
                        graphviz \
                        imagemagick \
                        postgresql-client-9.6 \
                        yarn \
                        nodejs \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && /usr/sbin/locale-gen &&\
    rm -rf /var/lib/apt/lists/*
```

We needed to use a specific folder of gems. We mounted this folder on runtime. It was a dev requirement to troubleshoot gem usage:

```Dockerfile
ENV GEM_HOME /gems/vendor
ENV GEM_SPEC_CACHE /gems/specs
ENV BUNDLE_APP_CONFIG /gems/vendor
ENV BUNDLE_PATH /gems/vendor
ENV BUNDLE_BIN /gems/vendor/bin
```

We needed to specify the app folder and create the app user to use root instead:

```Dockerfile
ENV APP_ROOT /app

WORKDIR $APP_ROOT/

RUN mkdir -p /gems

RUN groupadd -r app \
    && groupmod -g 1000 app \
    && useradd -g app -ms /bin/bash app \
    && chown app $APP_ROOT \ 
    && chown app /gems

USER app
```

We needed to install all ruby and node requirements:

```Dockerfile
COPY Gemfile.lock $APP_ROOT/
COPY Gemfile $APP_ROOT/
COPY package-lock.json $APP_ROOT/
COPY package.json $APP_ROOT/

RUN bundle install
RUN yarn install
```

In the end, we needed to specify the exposed port and default command:

```Dockerfile
EXPOSE 3000

CMD ["bundle","exec","rails","server","-b","0.0.0.0"]
```

## Adding docker-compose
We needed to use some external resources (i.e., DB), to that, Docker-compose was used on this setup.

```yaml
version: '3.4'
services:
  onboarding_app:
    working_dir: /app
    build:
      context: .
      args:
        RAILS_ENV: development
    env_file:
      - ./.env
    volumes:
      - .:/app
      - ./tmp/gems:/gems
      - onboarding_app_home:/home/app/
      - .irbrc:/home/app/.irbrc
    ports:
      - 3010:3000
    depends_on:
      - mailcatcher
      - postgres
      - redis
  mailcatcher:
    image: schickling/mailcatcher
    ports:
      - 1080:1080
  postgres:
    image: postgres:9.6-alpine
    ports:
      - 5432:5432
    volumes:
      - postgres:/var/lib/postgresql/data
  redis:
    image: redis:4.0.6-alpine
    volumes:
      - redis:/data
volumes:
  gems_2_5_1:
  postgres:
  redis:
  onboarding_app_home:
```

Run this command to bring up this environment:

```bash
docker-compose up --build
```

### Docker-compose file, explained

We needed to send the RAILS_ENV argument to build process of docker image. This RAILS_ENV variable was used to install all the gems related to development environment:

```yaml
build:
      context: .
      args:
        RAILS_ENV: development
```

We needed to inform the environment variable to be used on docker container execution:

```yaml
    env_file:
      - ./.env
```

We mounted some folders to help coding and troubleshooting. I will explain one by one.

First, we needed to mount the source code:

```yaml
    volumes:
      - .:/app
```

We needed to mount the folder used inside the docker image to add gems. This mounted folder is necessary to troubleshoot gem usage:

```yaml
      - ./tmp/gems:/gems
```

We needed to persist some config of app user so that we mounted the home folder of app user and .irbrc file too:

```yaml
      - onboarding_app_home:/home/app/
      - .irbrc:/home/app/.irbrc
```

To finish this service, we needed to publish the port used to connect on the application and inform which services it depends on:

```yaml
    ports:
      - 3010:3000
    depends_on:
      - mailcatcher
      - postgres
      - redis
```

These services are being used to provide external resources to app service:

```yaml
mailcatcher:
    image: schickling/mailcatcher
    ports:
      - 1080:1080
  postgres:
    image: postgres:9.6-alpine
    ports:
      - 5432:5432
    volumes:
      - postgres:/var/lib/postgresql/data
  redis:
    image: redis:4.0.6-alpine
    volumes:
      - redis:/data
```

Here we added the volumes used inside this docker-compose file:

```yaml
volumes:
  gems_2_5_1:
  postgres:
  redis:
  onboarding_app_home:
```

## To be continued...

## Thanks
My co-workers at [Paycertify](http://paycertify.com) who helped me giving me feedbacks and options to fix the problems, I need to say a special thanks to Rafael Affonso who enabled me to correct some mistakes about my English.