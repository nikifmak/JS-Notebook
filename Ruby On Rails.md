# Ruby On Rails

## Creating new project

```ruby
rails new Blog
```

## Database Config

The config for the database to be used, can be found on ``config/database.yml``.

By default the selected database is sqlite3 but ruby on rails supports multiple databases.

## Installing Sqlite3 on MacOsX

> ```shell
> brew install sqlite3
> ```

## Running Sqlite

> ```shell
> sqlite3
> ```

## Generating Models

We can generate models through `rails cli`

> ```ruby
> rails generate model Post title:string body:text
> ```

The generated model can be found on ``app/models/post.rb`` folder path:

>```ruby
>class Post < ApplicationRecord
>end
>```

The `<`  indicates that `Post` object inherits from `ApplicationRecord` object.

On folder path ``db/migrate`` we can see that equivalent migration file for our database ``20170826100417_create_posts.rb`` . To migrate to database means to take a programmaticaly defined object and create the equivalent object on a database.

## Migrating to Database.

Everytime we create a new model, it is good practice to push the changes to the database. This can be done with following command :

> ```shell
> rake db:migrate
> ```

In order to interact with the created database objects, we need to create **Controllers**.

## Creating Controllers

When we create the controllers we can define some actions. Let's see how is done: 

> ```shell
> rails generate controller Posts index new show destroy
> ```

The generated controller can be found on `app/controllers/posts_controller.rb`. But we can also see from the terminal that the upper command generated and routes !

>```shell
>route  get 'posts/destroy'
>route  get 'posts/show'
>route  get 'posts/new'
>route  get 'posts/index'
>```

Actually it generated more than that: 

> ```shell
> create  app/controllers/posts_controller.rb
>        route  get 'posts/destroy'
>        route  get 'posts/show'
>        route  get 'posts/new'
>        route  get 'posts/index'
>       invoke  erb
>       create    app/views/posts
>       create    app/views/posts/index.html.erb
>       create    app/views/posts/new.html.erb
>       create    app/views/posts/show.html.erb
>       create    app/views/posts/destroy.html.erb
>       invoke  test_unit
>       create    test/controllers/posts_controller_test.rb
>       invoke  helper
>       create    app/helpers/posts_helper.rb
>       invoke    test_unit
>       invoke  assets
>       invoke    coffee
>       create      app/assets/javascripts/posts.coffee
>       invoke    scss
>       create      app/assets/stylesheets/posts.scss
> ```

## Routes.rb

Then available route that are supported from our application can be found on `config/routes.rb`

> ```ruby
> Rails.application.routes.draw do
>   get 'posts/index'
>
>   get 'posts/new'
>
>   get 'posts/show'
>
>   get 'posts/destroy'
>
>   # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
> end
> ```

### How get the available routes from ror cli

> ```ruby
> rake routes
>
> Blog git:(master) ✗ rake routes
>        Prefix Verb URI Pattern              Controller#Action
>   posts_index GET  /posts/index(.:format)   posts#index
>     posts_new GET  /posts/new(.:format)     posts#new
>    posts_show GET  /posts/show(.:format)    posts#show
> posts_destroy GET  /posts/destroy(.:format) posts#destroy
> ```