+++
title = "How to deploy Ruby and Node app on Heroku using Docker"
date = "2019-02-15"
draft = false
Categories = ["english", "devops", "docker", "ruby", "node"]
Tags = ["english", "devops", "docker", "ruby", "node"]
+++

## LT;DR
I need to deploy a ruby+node application as a docker image on Heroku, but I won't use Heroku cli do build it.  This document is about how to test, build tag and deploy the docker image on a Heroku application according to the best practices.

## Attention
It is storytelling! If you copy and past some codes, without reading the context it won't work properly on your environment.

I will present the code and tell the story behind this idea, and I will show how I improved this code after seeing the problem.

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

This Dockerfile is responsible for setup everything that we need to start the development job.

We don't need to copy the code on build docker image phase. We will mount the source code inside the container on runtime:

```bash
docker build -t ruby_node_app:0.1 .
docker run -it --build-arg RAILS_ENV=development -v $PWD:/app ruby_node_app:0.1
```

The **RAILS_ENV** variable is used to change the gem installation behavior — some gems we don't need to install in production, for example.

### Dockefile, explained

We use this first section to set up the basis of this dev environment:

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

We need to use a specific folder of gems. We will mount this folder in the future. It is a dev requirement to troubleshoot gem usage:

```Dockerfile
ENV GEM_HOME /gems/vendor
ENV GEM_SPEC_CACHE /gems/specs
ENV BUNDLE_APP_CONFIG /gems/vendor
ENV BUNDLE_PATH /gems/vendor
ENV BUNDLE_BIN /gems/vendor/bin
```

We need to specify the app folder and create the app user to use root instead:

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

We need to install all ruby and node requirements:

```Dockerfile
COPY Gemfile.lock $APP_ROOT/
COPY Gemfile $APP_ROOT/
COPY package-lock.json $APP_ROOT/
COPY package.json $APP_ROOT/

RUN bundle install
RUN yarn install
```

In the end, we need to specify expose port and default command:

```Dockerfile
EXPOSE 3000

CMD ["bundle","exec","rails","server","-b","0.0.0.0"]
```

## Adding docker-compose
We need to use some external resource (i.e., DB), to that, we will introduce docker-compose usage on this setup.

```Dockerfile
version: '3.4'
services:
  onboarding_app:
    working_dir: /app
    build:
      context: .
      args:
        RAILS_ENV: development
    image: onboarding_app:development
    user: app
    env_file:
      - ./.env
    command: bundle exec rails server -b 0.0.0.0
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
    tty: true
    stdin_open: true
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

We need to 

```Dockerfile
build:
      context: .
      args:
        RAILS_ENV: development
```