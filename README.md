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
- [ ] Devise version 4.3

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

## For more information

- [ ] [Follow this video](https://www.youtube.com/watch?v=5gUysPm64a4&t=1629s).

## License

- [ ] See [LICENSE.md](LICENSE.md) for details.

## Authors

- [ ] [Muzykina Anna](https://github.com/Anna-Myzukina)
