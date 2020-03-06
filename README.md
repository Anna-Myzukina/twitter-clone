## Project: Twitter clone

The app itself will feature a basic CRUD principle where we can:
* create, 
* read, 
*update, 
* and destroy Tweeets. 
On top of the Tweeets, I used gem called Devise which makes creating an entire user role and authentication system easy. Combined with this gem we can authenticate users who want to author Tweeets. A user's Tweeets are then also tied to their account. The end result is a public facing site with a stream of tweets from different users. Users that have an account can login to create their own Tweeets to add to the public stream.

## Environment

- [ ] Ruby on Rails version 5.1.4
- [ ] bcrypt version 3.1.7 ([bcrypt()](https://github.com/codahale/bcrypt-ruby) allows you to easily harden your application against these kinds of attacks.)
- [ ] I used [bulma](https://bulma.io/) instead of [bootstrap-sass](https://www.rubydoc.info/gems/bootstrap-sass/3.3.6) 

- [ ] [better_errors](https://rubygems.org/gems/better_errors/versions/2.1.1) version 2.4, this gem provides a better error page for Rails and other Rack apps. Includes source code inspection, a live REPL and local/instance variable inspection for all stack frames.
- [ ] [simple_form](https://rubygems.org/gems/simple_form) version 3.5 don`t forgot to visit home page of [simple_form](https://github.com/heartcombo/simple_form) here instructions and run next command if you don`t use bootastrap:

    rails generate simple_form:install

- [ ] [gravatar_image_tag](https://rubygems.org/gems/gravatar_image_tag) versio 1.2 .A configurable and documented Rails view helper for adding gravatars into your Rails application.

- [ ] [Devise](https://rubygems.org/gems/devise) version 4.3 .Flexible authentication solution for Rails with Warden. Devise [home page](https://github.com/heartcombo/devise)

    rails generate devise:install

At this point, a number of instructions will appear in the console. Among these instructions, you'll need to set up the default URL options for the Devise mailer in each environment. Here is a possible configuration for config/environments/development.rb:

    config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }


After we should add next to views/layouts/application.html.erb :

        <% if flash[:notice] %>
        <div class="notification is-primary global-notification">
        <p class="notice"><%= notice %></p>
        </div>
        <% end %>
        <% if flash[:alert] %>
        <div class="notification is-danger global-notification">
        <p class="alert"><%= alert %></p>
        </div>
        <% end %>

And run next command:

    rails g devise:views

    rails g devise User

    rails db:migrate



Next step its authentification 

add next in controller/tweeets_controller.rb

    before_action :authenticate_usr!, except: [:index, :show]

add in controller/registration_controller.rb

        class RegistrationsController < Devise::RegistrationsController

        private

        def sign_up_params
            params.require(:user).permit(:name, :username, :email, :password, :password_confirmation)

        end

        def acount_update_params
            params.require(:user).permit(:name, :username, :email, :password, :password_confirmation, :current_password)
        end
        end


After run command:

    rails g migration AddFieldsToUsers

Then add next to db/migrate/xxxxxxxxxxx_add_fields_to_user.rb

        class AddFieldsToUsers < ActiveRecord::Migration[5.1]
        def change
            add_column :users, :name, :string
            add_column :users, :username, :string
            add_index :users, :username, unique: true 
        end
        end

After run next command:

    rails db:migrate
    
## Header:

add next to views/layouts/application.html.erb 

        <nav class="navbar is-info">
        <div class="navbar-brand">
        <%= link_to root_path, class: "navbar-item" do %>
        <h1 class="title is-5">twittter</h1>
        <% end %>
            <div class="navbar-burger burger" data-target="navbarExample">
            <span></span>
            <span></span>
            <span></span>

        </div>
        </div>

        <div id="navbarExample" class="navbar-menu">
        <div class="navbar-end">
            <div class="field is-grouped">
            <p class="control">
        <%= link_to 'New Tweeet', new_tweeet_path, class:"button is-info is-inverted"
        %>
                </p>
                <p class="control">Sign Up</p>
                    </div>
                </div>
                </div>
            </nav>
            <%= yield %>

## Getting started

To get started with the app, clone the repo and then install the needed gems:

```
$ bundle install --without production
```

Next, migrate the database:

```
$ rails db:migrate
```

Finally, run the test suite to verify that everything is working correctly:

```
$ rails test
```

If the test suite passes, you'll be ready to run the app in a local server:

```
$ rails server
```

## Tesing with rspec

* [Testing with RSpec .](https://medium.com/@gracemugoiri/testing-with-rspec-4e8fb6dd7057)

## For more information

- [ ] [Follow this video](https://www.youtube.com/watch?v=5gUysPm64a4&t=1629s).

## License

- [ ] See [LICENSE.md](LICENSE.md) for details.

## Authors

- [ ] [Muzykina Anna](https://github.com/Anna-Myzukina)
