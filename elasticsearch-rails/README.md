# Elasticsearch::Rails

The `elasticsearch-rails` library is a companion for the
the [`elasticsearch-model`](https://github.com/elasticsearch/elasticsearch-rails/tree/master/elasticsearch-model)
library, providing features suitable for Ruby on Rails applications.

The library is compatible with Ruby 1.9.3 and higher.

## Installation

Install the package from [Rubygems](https://rubygems.org):

    gem install elasticsearch-rails

To use an unreleased version, either add it to your `Gemfile` for [Bundler](http://bundler.io):

    gem 'elasticsearch-rails', git: 'git://github.com/elasticsearch/elasticsearch-rails.git'

or install it from a source code checkout:

    git clone https://github.com/elasticsearch/elasticsearch-rails.git
    cd elasticsearch-rails/elasticsearch-rails
    bundle install
    rake install

## Features

### Rake Tasks

To facilitate importing data from your models into Elasticsearch, require the task definition in your application,
eg. in the `lib/tasks/elasticsearch.rake` file:

```ruby
require 'elasticsearch/rails/tasks/import'
```

To import the records from your `Article` model, run:

```bash
$ bundle exec rake environment elasticsearch:import:model CLASS='Article'
```

Run this command to display usage instructions:

```bash
$ bundle exec rake -D elasticsearch
```

### ActiveSupport Instrumentation

To display information about the search request (duration, search definition) during development,
and to include the information in the Rails log file, require the component in your `application.rb` file:

```ruby
require 'elasticsearch/rails/instrumentation'
```

You should see an output like this in your application log in development environment:

    Article Search (321.3ms) { index: "articles", type: "article", body: { query: ... } }

Also, the total duration of the request to Elasticsearch is displayed in the Rails request breakdown:

    Completed 200 OK in 615ms (Views: 230.9ms | ActiveRecord: 0.0ms | Elasticsearch: 321.3ms)

There's a special component for the [Lograge](https://github.com/roidrage/lograge) logger.
Require the component in your `application.rb` file (and set `config.lograge.enabled`):

```ruby
require 'elasticsearch/rails/lograge'
```

You should see the duration of the request to Elasticsearch as part of each log event:

    method=GET path=/search ... status=200 duration=380.89 view=99.64 db=0.00 es=279.37

### Rails Application Templates

You can generate a fully working example Ruby on Rails application, with an `Article` model and a search form,
to play with (it even downloads _Elasticsearch_ itself, generates the application skeleton and leaves you with
a _Git_ repository to explore the steps and the code):

```bash
rails new searchapp --skip --skip-bundle --template https://raw.github.com/elasticsearch/elasticsearch-rails/master/elasticsearch-rails/lib/rails/templates/01-basic.rb
```

Run the same command again, in the same folder, with the `02-pretty` template to add features such as
a custom `Article.search` method, result highlighting and [_Bootstrap_](http://getbootstrap.com) integration:

```bash
rails new searchapp --skip --skip-bundle --template https://raw.github.com/elasticsearch/elasticsearch-rails/master/elasticsearch-rails/lib/rails/templates/02-pretty.rb
```

NOTE: A third, much more complex template, demonstrating other features such as faceted navigation or
      query suggestions is being worked on.

## TODO

This is an initial release of the `elasticsearch-rails` library. Many more features are planned and/or
being worked on, such as:

* Rake tasks for convenient (re)indexing your models from the command line
* Hooking into Rails' notification system to display Elasticsearch related statistics in the application log
* Instrumentation support for NewRelic integration

## License

This software is licensed under the Apache 2 license, quoted below.

    Copyright (c) 2014 Elasticsearch <http://www.elasticsearch.org>

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
