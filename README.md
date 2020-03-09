## Project: Twitter clone

The app itself will feature a basic CRUD principle where we can:
* create, 
* read, 
*update, 
* and destroy Tweeets. 
On top of the Tweeets, I used gem called Devise which makes creating an entire user role and authentication system easy. Combined with this gem we can authenticate users who want to author Tweeets. A user's Tweeets are then also tied to their account. The end result is a public facing site with a stream of tweets from different users. Users that have an account can login to create their own Tweeets to add to the public stream.

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

And run next commands:

* first

    `rails g devise:views`

* second

    `rails g devise User`

* third

    `rails db:migrate`



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

## Sign up form

Replace code in app/views/devise/registrations/new.html.erb

        <section class="section">
          <div class="container">
            <div class="columns is-centered">

                <div class="column is-4">
                <h2 class="title is-2">Sign Up</h2>

            <%= simple_form_for(resource, as: resource_name, url: registration_path(resource_name)) do |f| %>
            <%= f.error_notification %>


            <div class="field">
                <div class="control">
                    <%= f.input :email,
                            required: true,
                            autofocus: true,
                            input_html: { class: "input" },
                            wrapper: false,
                            label_html: {class: "label"} %>
                </div>
            </div>              

            <div class="field">
                <div class="control">
                    <%= f.input :password,
                            required: true,
                            hint: ("#{@minimum_password_length} characters minimum" if @minimum_password_length),
                            input_html: { class: "input" },
                            wrapper: false,
                            label_html: {class: "label"} %>
                </div>
            </div>
            
            <div class="field">
                <div class="control">
                    <%= f.input :password_confirmation,
                            required: true,
                            input_html: { class: "input" },
                            wrapper: false,
                            label_html: {class: "label"} %>
                </div>
            </div>

            <div class="field">
                <div class="control">
                <%= f.button :submit, "Sign up", class:"button is-info is-medium" %>
                </div>
            </div>
            <% end %>
            <br />
            <%= render "devise/shared/links" %>
                </div>
                </div>
            </div>
            </section>


Replace code in app/views/devise/registrations/edit.html.erb

        <section class="section">
        <div class="container">
            <div class="columns is-centered">
            <div class="column is-4">
                
            <h2 class="title is-2">Edit <%= resource_name.to_s.humanize %></h2>
            <%= simple_form_for(resource, as: resource_name, url: registration_path(resource_name), html: { method: :put }) do |f| %>
                <%= f.error_notification %>

                <div class="field">
                    <div class="control">
                    <%= f.input :name, required: true, autofocus: true, input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
                    </div>
                </div>

                <div class="field">
                    <div class="control">
                    <%= f.input :username, required: true,  input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
                    </div>
                </div>

                <div class="field">
                    <div class="control">
                    <%= f.input :email, required: true, input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
                    </div>
                </div>

                <div class="field">
                <% if devise_mapping.confirmable? && resource.pending_reconfirmation? %>
                    <p>Currently waiting confirmation for: <%= resource.unconfirmed_email %></p>
                <% end %>
                </div>

                <div class="field">
                    <div class="control">
                    <%= f.input :password, autocomplete: "off", hint: "leave it blank if you don't want to change it", required: false, input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
                    </div>
                </div>

                <div class="field">
                    <div class="control">
                    <%= f.input :password_confirmation, required: false, input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
                    </div>
                </div>

                <div class="field">
                    <div class="control">
                    <%= f.input :current_password, hint: "we need your current password to confirm your changes", required: true, input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
                    </div>
                </div>
        
                <%= f.button :submit, "Update", class:"button is-info" %>
            
            <% end %>

                <hr />
                <h3 class="title is-5">Cancel my account</h3>
                <p>Unhappy? <%= link_to "Cancel my account", registration_path(resource_name), data: { confirm: "Are you sure?" }, method: :delete %></p>
        
            </div>
            </div>
        </div>
        </section>

## Log in form

In app/views/devise/sessions/new.html.erb


            <section class="section">
            <div class="container">
                <div class="columns is-centered">

                <div class="column is-4">
                <h2 class="title is-2">Log in</h2>

            <%= simple_form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
                <div class="field">
                <div class="control">
                    <%= f.input :email,
                            required: false,
                            autofocus: true,
                            input_html: { class: "input" },
                            wrapper: false,
                            label_html: {class: "label"} %>
                </div>
                </div>

                <div class="field">
                <div class="control">
                    <%= f.input :password,
                            required: false,
                            input_html: { class: "input" },
                            wrapper: false,
                            label_html: {class: "label"} %>
                </div>
                </div>

                <div class="field">
                <div class="control">
                    <%= f.input :remember_me, wrapper: false, as: :boolean if devise_mapping.rememberable? %>
                </div>
                </div>


                    <%= f.button :submit, "Log in", class: "button is-info is-medium" %>
            <% end %>
            <br />
            <%= render "devise/shared/links" %>

                </div>
                </div>
            </div>
            </section>

## Users relationship

Add next code to app/models/user.rb

    has_many :tweeets

app/models/tweeet.rb

    belongs_to :user

After let`s add migration
run first command

    rails g migration AddUserIdToTweeets user_id:integer

run second command

    rails db:migrate

## Testing our migration

Open console by running next command

    rails c

To test our users let`s type

    User.connection

    @user = User

Should be next output:

        irb(main):005:0> @user = User
        => User(id: integer, email: string, encrypted_password: string, reset_password_token: string, reset_password_sent_at: datetime, remember_created_at: datetime, created_at: datetime, updated_at: datetime, name: string, username: string)

After type

    @tweeet = Tweeet

Should be next output:

    irb(main):004:0> @tweeet = Tweeet
    => Tweeet(id: integer, tweeet: text, created_at: datetime, updated_at: datetime, user_id: integer) 

    If you get the same output my congratulation your migration is working you can run `exit`


## For more information

- [ ] [Follow this video](https://www.youtube.com/watch?v=5gUysPm64a4&t=1629s).

## License

- [ ] See [LICENSE.md](LICENSE.md) for details.

## Authors

- [ ] [Muzykina Anna](https://github.com/Anna-Myzukina)
