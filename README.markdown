# Delayed::Web for Delayed::Job

[![GitHub release](https://img.shields.io/github/release/thebestday/delayed-web.svg)](https://github.com/thebestday/delayed-web/releases/latest)
[![Build Status](https://travis-ci.org/thebestday/delayed-web.png?branch=master)](https://travis-ci.org/thebestday/delayed-web)
[![Code Climate](https://codeclimate.com/github/thebestday/delayed-web.png)](https://codeclimate.com/github/thebestday/delayed-web)

A rails engine that provides a simple web interface for exposing the
[Delayed::Job](https://github.com/collectiveidea/delayed_job) queue.

![](http://f.cl.ly/items/1j2m2g223s3q33140S3H/Screen%20Shot%202013-06-13%20at%2011.47.57%20PM.png)

## Requirements

* Ruby 1.9.3, 2.0.0, 2.1.0 or 2.2.0
* Rails 3.2.x, 4.0.x, 4.1.x or 4.2.x
* Delayed::Job 3.0.x, 4.0.x
* ActiveRecord or Mongoid

## Installation

Put this in your Gemfile.

    gem 'delayed-web'

Run bundler.

    bundle install

Run the installation generator.

    rails generate delayed:web:install

This will install an initializer into `config/initializers/delayed_web.rb`,
mount the engine at `/jobs` in `config/routes.rb` and add the stylesheets
to the list of precomiled assets in `application.rb` (If assets are
enabled).

## Routes

The installation script will automatically add routes into your app,
like this.

    # config/routes.rb
    Rails.application.routes.draw do
      mount Delayed::Web::Engine, at: '/jobs'
    end

This will mount the following routes.

    GET /
    GET /jobs
    GET /jobs/:id

## Authentication

Out of the box Delayed::Web doesn't make any assumptions about
authentication. It's possible to take advantage of rails routing
constraints or rack middleware.

An example using Devise's `authenticated` constraint.

    # config/routes.rb
    authenticated :user, -> user { user.admin? } do
      mount Delayed::Web::Engine, at: '/jobs'
    end

An example using Rack::Auth::Basic.

    # config/routes.rb
    Delayed::Web::Engine.middleware.use Rack::Auth::Basic do |username, password|
      username == ENV['USERNAME'] && password == ENV['PASSWORD']
    end

## Backend

The installation script will automatically set the backend you're using for
Delayed::Job, like this.

    # config/initializers/delayed_web.rb
    Rails.application.config.to_prepare do
      Delayed::Web::Job.backend = 'active_record'
    end

Currently supported backends are `'active_record'` and `mongoid'`. Patches
are always welcome.

## Asset Pipeline

The installation script will automatically add the engine's assets to your
application's list of precompiled assets.

    # config/application.rb
    config.assets.enabled = true
    config.assets.precompile << 'delayed/web/application.css'

This step is skipped if your project is configured not to use the asset
pipeline.

## Developing Locally

After you've cloned the repository, run bundler.

    bundle install

Make sure the specs are passing.

    rake

Run the dummy rails app.

    cd spec/dummy
    rails s

Go to http://localhost:3000.

The dummy rails app uses `Delayed::Web::Job::Double` as its backend.
It is an in-memory double of a real Delayed::Job.

## Special Thanks

We're grateful for all our [contributors](https://github.com/thebestday/delayed-web/graphs/contributors),
but we're especially grateful for contributions by the following people:

* [@coop](https://github.com/coop)
* [@eltiare](https://github.com/eltiare)

## License

Copyright © 2013-2015 TheBestDay Pty. Ltd. Released under the MIT license. See LICENSE.
