# Лабораторна робота №2

**Тема: Створення і знайомство з першим Rails застосунком**

**Мета: Навчитися розробляти власні web-додатки**

## 0. The many advantages of Rails

Ruby on Rails (or just “Rails” for short) is a free and open-source web development framework written in the Ruby programming language. Upon its debut, Ruby on Rails rapidly became one of the most popular tools for building dynamic web applications. Rails is used by companies as varied as Airbnb, SoundCloud, Disney, Hulu, GitHub, and Shopify, as well as by innumerable freelancers, independent development shops, and startups.

Although there are many choices in web development, Rails stands apart for its elegance, power, and integrated approach to web applications. Using Rails, even novice developers can build a full-stack web application without ever leaving the framework—a huge boon for people learning web development for the first time. Rails also gives you flexibility going forward—for example, serving as a great back end if you want to build a single-page application or mobile app sometime down the line.

## 1. Installing Rails

Verify that you have a current version of Ruby installed:
```sh
$ ruby -v
```

Many popular UNIX-like OSes ship with an acceptable version of SQLite3.
On Windows, if you installed Rails through Rails Installer, you already have SQLite installed.
Others can find installation instructions at the SQLite3 website.
Verify that it is correctly installed and in your PATH:

```sh
$ sqlite3 --version
```
The program should report its version.

To install Rails, we’ll use the gem command provided by the [RubyGems](https://rubygems.org/) package manager:
```sh
$ gem install rails
```

To verify that you have everything installed correctly, you should be able to run the following:
```sh
$ rails --version
```

## 2.Creating a web application

1. Go to the appropriate directory, where the folder with the project will be created, using the `cd` command.
2. Run the command to create application:
```sh
$ rails new <application name> --skip-bundle
      create
      create  README.md
      create  Rakefile
      create  .ruby-version
      create  config.ru
      create  .gitignore
      create  Gemfile
      run  git init from "."
      Initialized empty Git repository in /home/ubuntu/environment/hello_app/.git/
      create  package.json
      create  app
      create  app/assets/config/manifest.js
      create  app/assets/stylesheets/application.css
      create  app/channels/application_cable/channel.rb
      create  app/channels/application_cable/connection.rb
      create  app/controllers/application_controller.rb
      create  app/helpers/application_helper.rb
      .
      .
      .
```

Notice how many files and directories the rails command creates.
This standard directory and file structure is one of the many advantages of Rails: it immediately gets you from zero to a functional (if minimal) application.
Moreover, since the structure is common to all Rails apps, you can immediately get your bearings when looking at someone else’s code.

| File/Directory  | Purpose                                                                             |
|-----------------|-------------------------------------------------------------------------------------|
| `app/`          | core application (app) code, including models, views, controllers, and helpers      |
| `app/assets`    | application assets such as Cascading Style Sheets (CSS) and images                  |
| `bin/`          | binary executable files                                                             |
| `config/`       | application configuration                                                           |
| `db/`           | database files                                                                      |
| `doc/`          | documentation for the application                                                   |
| `lib/`          | library modules                                                                     |
| `log/`          | application log files                                                               |
| `public/`       | data accessible to the public (e.g., via web browsers), such as error pages         |
| `bin/rails`     | a program for generating code, opening console sessions, or starting a local server |
| `test/`         | application tests                                                                   |
| `tmp/`          | temporary files                                                                     |
| `README.md`     | a brief description of the application                                              |
| `Gemfile`       | gem requirements for this app                                                       |
| `Gemfile.lock`  | a list of gems used to ensure that all copies of the app use the same gem versions  |
| `config.ru`     | a configuration file for Rack middleware                                            |
| `.gitignore`    | patterns for files that should be ignored by Git                                    |



After creating a new Rails application, the next step is to use Bundler to install and include the gems needed by the app.
A `Gemfile` was automatically created by the rails new command, you can look at this file with a text editor.

Many of these lines are commented out with the hash symbol #; they are there to show you some commonly needed gems and to give examples of the Bundler syntax.
For now, we won’t need any gems other than the defaults.

Unless you specify a version number to the gem command, Bundler will automatically install the latest requested version of the gem.
This is the case, for example, in the code:
```rb
gem "sprockets-rails"
```

There are also two common ways to specify a gem version range, which allows us to exert some control over the version used by Rails. The first looks like this:

```rb
gem "capybara", ">= 3.26"
```

This would install the latest version of the `capybara` gem (which is used in testing) as long as it’s greater than or equal to version `3.26`—even if it’s, say, version `7.2`.

The second method looks like this:
```rb
gem "sqlite3", "~> 1.4"
```

This installs the gem `sqlite3` as long as it’s version `1.4` or newer (a “minor update”) but not `2` or newer (a “major update”). In other words, the `>=` notation always installs the latest gem as long as it meets the minimum version requirement, whereas the `~> 1.4` notation will install `1.5` (if available) but not `2.0`

The main idea behind using `~>` is that it should generally be safe to use the latest minor update of a gem, but experience shows that even minor point releases can break Rails applications.
As a result, we’ll err on the side of caution by including exact version numbers for all gems.
You are welcome to use the most up-to-date version of any gem, including using the `~>` construction in the `Gemfile` (which I generally recommend for more advanced users), but be warned that this may cause the tutorial to act unpredictably.

Install the gems using `bundle install` command:
```sh
$ cd ./<application name>/
$ bundle install
      Fetching source index for https://rubygems.org/
      .
      .
      .
```
The `bundle install` command might take a few moments, but when it’s done our application will be ready to run.

Thanks to running `rails new` and `bundle install`, we already have an application we can run — but how? Happily, Rails comes with a command-line program, or script, that runs a local web server to assist us in developing our application:
```sh
$ rails server
      => Booting Puma
      => Ctrl-C to shutdown server
```

To view the result of `rails server`, paste the URL http://localhost:3000 into the address bar of your browser.
You should see the Rails Welcome page:
![image](./images/2.png)

## 3. Model-View-Controller (MVC)
Even at this early stage, it’s helpful to get a high-level overview of how Rails applications work, as illustrated in Figure below.

![image](./images/3.png)

You might have noticed that the standard Rails application structure has an application directory called `app/`, which includes subdirectories called models, views, and controllers (among others).
This is a hint that Rails follows the model-view-controller (MVC) architectural pattern, which enforces a separation between the data in the application (such as user information) and the code used to display it, which is a common way of structuring a graphical user interface (GUI).

When interacting with a Rails application, a browser sends a request, which is received by a web server and passed on to a Rails controller, which is in charge of what to do next.
In some cases, the controller will immediately render a view, which is a template that gets converted to HTML and sent back to the browser.
More commonly for dynamic sites, the controller interacts with a model, which is a Ruby object that represents an element of the site (such as a user) and is in charge of communicating with the database.
After invoking the model, the controller then renders the view and returns the complete web page to the browser as HTML.

## 4. Hello World!

As a first application of the MVC framework, we’ll make a wafer-thin change to the first app by adding a controller action to render the string “Hello world!” to replace the default Rails page.

As implied by their name, controller actions are defined inside controllers.
We’ll call our action `hello` and place it in the Application controller.
Indeed, at this point the Application controller is the only controller we have, which you can verify by running
```sh
$ ls app/controllers/*_controller.rb
```
to view the current controllers.

Add a `hello` action to the `ApplicationController` in `app/controllers/application_controller.rb`
```rb
class ApplicationController < ActionController::Base
  def hello
    render html: "Hello world!"
  end
end
```

Having defined an action that returns the desired string, we need to tell Rails to use that action instead of the default page.
To do this, we’ll edit the Rails _router_, which sits in front of the controller and determines where to send requests that come in from the browser.
In particular, we want to change the default page, the _root route_, which determines the page that is served on the root URL.
Because it’s the URL for an address like http://www.example.com/ (where nothing comes after the final forward slash), the root URL is often referred to as `/` (“slash”) for short.


The Rails routes file (`config/routes.rb`) includes a comment directing us to the Rails Guide on Routing and includes an example of how to define the root route.
The syntax looks like this:
```rb
root "controller_name#action_name"
```

In the present case, the controller name is `application` and the action name is `hello`, which results in the code.
Set the root route:
```rb
Rails.application.routes.draw do
  root "application#hello"
end
```

With those changes, the root route returns “Hello world!” as required.
![image](./images/4.png)

**Lets practice:** By following the example of the `hello` action, add a second action called `goodbye` that renders the text “goodbye, world!”. Edit the routes file, so that the root route goes to goodbye instead of to hello.

## 5. A toy app

### 5.1. Planning the application

Now we’re ready to start making the app itself. The typical first step when making a web application is to create a data model, which is a representation of the structures needed by our application, including the relationships between them. In our case, the toy app will be a Twitter-style microblog, with only users and short (micro)posts. Thus, we’ll begin with a model for users of the app in this section, and then we’ll add a model for microposts.

#### 5.1.1. A toy model for users

There are as many choices for a user data model as there are different registration forms on the Web; for simplicity, we’ll go with a distinctly minimalist approach. Users of our toy app will have a unique identifier called `id` (of type `integer`), a publicly viewable `name` (of type `string`), and an `email address` (also of type `string`) that will double as a unique username. (Note that there is no password attribute at this point, which is part of what makes this app a “toy”) A summary of the data model for users you can see below.

![image](./images/5.1.1.png)

#### 5.1.2. A toy model for microposts

Recall from the introduction that a micropost is simply a short post, essentially a generic term for the brand-specific “tweet” (with the prefix “micro” motivated by Twitter’s original description as a “micro-blog”). The core of the micropost data model is even simpler than the one for users: a micropost has only an `id` and a `content` field for the micropost’s text (of type `text`) There’s an additional complication, though: we want to associate each micropost with a particular user. We’ll accomplish this by recording the `user_id` of the owner of the post.

![image](./images/5.1.2.png)

### 5.2. The Users resource

In this section, we’ll implement the users data model in Section 5.1.1, along with a web interface to that model. The combination will constitute a _Users resource_, which will allow us to think of users as objects that can be created, read, updated, and deleted through the web via the HTTP protocol. As promised in the introduction, our Users resource will be created by a scaffold generator program, which comes standard with each Rails project. I urge you not to look too closely at the generated code; at this stage, it will only serve to confuse you.

Rails scaffolding is generated by passing the `scaffold` command to the `rails generate` script. The argument of the `scaffold` command is the singular version of the resource name (in this case, `User`), together with optional parameters for the data model’s attributes:
```sh
$ rails generate scaffold User name:string email:string
      invoke  active_record
      create    db/migrate/<timestamp>_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/models/user_test.rb
      create      test/fixtures/users.yml
      invoke  resource_route
       route    resources :users
      invoke  scaffold_controller
      create    app/controllers/users_controller.rb
      invoke    erb
      create      app/views/users
      create      app/views/users/index.html.erb
      create      app/views/users/edit.html.erb
      create      app/views/users/show.html.erb
      create      app/views/users/new.html.erb
      create      app/views/users/_form.html.erb
      invoke    test_unit
      create      test/controllers/users_controller_test.rb
      create      test/system/users_test.rb
      invoke    helper
      create      app/helpers/users_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/users/index.json.jbuilder
      create      app/views/users/show.json.jbuilder
      create      app/views/users/_user.json.jbuilder
```

By including `name:string` and `email:string`, we have arranged for the User model to have the form shown in Figure from Section 5.1.1. (Note that there is no need to include a parameter for `id`; it is created automatically by Rails for use as the primary key in the database.)

To proceed with the toy application, we first need to migrate the database using `rails db:migrate`:
```sh
$ rails db:migrate
== CreateUsers: migrating ======================================
-- create_table(:users)
   -> 0.0027s
== CreateUsers: migrated (0.0036s) =============================
```

The effect of it is to update the database with our new users data model. 

#### 5.2.1. A user tour

In generating the Users resource scaffolding in Section above, Rails created a large number of pages for manipulating users. For example, the page for listing all users is at `/users`, and the page for making a new user is at `/users/new`. The rest of this section is dedicated to taking a whirlwind tour through these user pages. Table shows the correspondence between pages and URLs:

| URL             | Action  | Purpose                     |
|-----------------|---------|-----------------------------|
| `/users`        | `index` | page to list all users      |
| `/users/1`      | `show`  | page to show user with id 1 |
| `/users/new`    | `new`   | page to make a new user     |
| `/users/1/edit` | `edit`  | page to edit user with id 1 |


We start with the page to show all the users in our application, called index and located at `/users`. To navigate to this page, click in the address bar of your browser and add “/users” to the end of the root URL so that the full URL is of the form http://localhost:3000/users.

As you might expect, initially there are no users at all.
![image](./images/5.2.1_1.png)

To make a new user, we can click on the New User link to visit the `new` page at `/users/new`, as shown below:
![image](./images/5.2.1_2.png)

We can create a user by entering name and email values in the text fields and then clicking the Create User button.
The result is the user `show` page at `/users/1` (The green welcome message is accomplished using the flash).
Note that the URL is `/users/1`; as you might suspect, the number `1` is simply the user’s `id` attribute.
This page will become the user’s profile page.

![image](./images/5.2.1_3.png)

To change a user’s information, we click the Edit link to visit the `edit` page at `/users/1/edit`.
By modifying the user information and clicking the Update User button, we arrange to change the information for the user in the toy application.
![image](./images/5.2.1_4.png)

![image](./images/5.2.1_5.png)

Now we’ll create a second user by revisiting the `new` page at `/users/new` and submitting a second set of user information.
The resulting user `index` is shown below:

![image](./images/5.2.1_6.png)


Having shown how to `create`, `show`, and `edit` users, we come finally to destroying them. You should verify that clicking on the “Destroy this user” button destroys the second user, yielding an index page with only one user. Section 10.4 adds user deletion to the sample app, taking care to restrict its use to a special class of administrative users.

#### 5.2.2. MVC in action

Now that we’ve completed a quick overview of the Users resource, let’s examine one particular part of it in the context of the model-view-controller (MVC) pattern introduced before. Our strategy will be to describe the results of a typical browser hit—a visit to the user index page at _/users_—in terms of MVC

![image](./images/5.2.2.png)

Here is a summary of the steps shown in Figure above

1. The browser issues a request for the `/users` URL.
2. Rails routes `/users` to the `index` action in the Users controller.
3. The `index` action asks the User model to retrieve all users (`User.all`).
4. The User model pulls all the users from the database.
5. The User model returns the list of users to the controller.
6. The controller captures the users in the `@users` variable, which is passed to the `index` view.
7. The view uses embedded Ruby to render the page as HTML.
8. The controller passes the HTML back to the browser.

Now let’s take a look at the above steps in more detail.
We start with a request issued from the browser—i.e., the result of typing a URL in the address bar or clicking on a link (**Step 1**).
This request hits the Rails router (**Step 2**), which dispatches the request to the proper controller action based on the URL.
The code to create the mapping of user URLs to controller actions for the Users **resource** appears in listing below. This code effectively sets up the table of URL/action pairs.

The Rails routes, with a rule for the Users resource (./config/routes.rb).
```rb
Rails.application.routes.draw do
  resources :users
  root 'application#hello'
end
```

While we’re looking at the routes file, let’s take a moment to associate the root route with the users index, so that “slash” goes to /users.
In the present case, we want to use the index action in the Users controller, which we can arrange using the code shown below (./config/routes.rb):
```rb
Rails.application.routes.draw do
  resources :users
  root 'users#index'
end
```

A controller contains a collection of related actions, and the pages from the tour in Section 5.2.1 correspond to actions in the Users controller. The controller generated by the scaffolding is shown schematically in Listing below. Note the code `class UsersController < ApplicationController`, which is an example of a Ruby class with inheritance.


```rb
class UsersController < ApplicationController
  ...

  def index
    ...
  end

  def show
    ...
  end

  def new
    ...
  end

  def edit
    ...
  end

  def create
    ...
  end

  def update
    ...
  end

  def destroy
    ...
  end
end
```


You might notice that there are more actions than there are pages; the `index`, `show`, `new`, and `e`dit actions all correspond to pages from Section 5.2.1, but there are additional `create`, `update`, and `destroy` actions as well. These actions don’t typically render pages (although they can); instead, their main purpose is to modify information about users in the database.

This full suite of controller actions, summarized in Table below, represents the implementation of the REST architecture in Rails. Note from Table that there is some overlap in the URLs; for example, both the user `show` action and the `update` action correspond to the URL `/users/1`. The difference between them is the **HTTP** request method they respond to.

| HTTP request method | URL           | Action    | Purpose                     |
|---------------------|---------------|-----------|-----------------------------|
| GET                 | /users        | `index`   | page to list all users      |
| GET                 | /users/1      | `show`    | page to show user with id 1 |
| GET                 | /users/new    | `new`     | page to make a new user     |
| POST                | /users        | `create`  | create a new user           |
| GET                 | /users/1/edit | `edit`    | page to edit user with id 1 |
| PATCH               | /users/1      | `update`  | update user with id 1       |
| DELETE              | /users/1      | `destroy` | delete user with id 1       |


> If you read much about Ruby on Rails web development, you’ll see a lot of references to “REST”, which is an acronym for REpresentational State Transfer. REST is an architectural style for developing distributed, networked systems and software applications such as the World Wide Web and web applications. Although REST theory is rather abstract, in the context of Rails applications REST means that most application components (such as users and microposts) are modeled as resources that can be created, read, updated, and deleted—operations that correspond both to the CRUD operations of relational databases and to the four fundamental HTTP request methods: POST, GET, PATCH, and DELETE. 
> 
> As a Rails application developer, the RESTful style of development helps you make choices about which controllers and actions to write: you simply structure the application using resources that get created, read, updated, and deleted. In the case of users and microposts, this process is straightforward, since they are naturally resources in their own right. 


To examine the relationship between the Users controller and the User model, let’s focus on the index action, shown

The simplified user index action for the toy application (./app/controllers/users_controller.rb)

```rb
class UsersController < ApplicationController
  ...

  def index
    @users = User.all
  end
  ...
end
```

This `index` action has the line `@users = User.all` (**Step 3** in Figure Section 5.2.2), which asks the User model to retrieve a list of all the users from the database (**Step 4**), and then places them in the variable `@users` (pronounced “at-users”) (**Step 5**).


The User model itself appears in Listing belo. Although it is rather plain, it comes equipped with a large amount of functionality because of inheritance. In particular, by using the Rails library called Active Record, the code arranges for `User.all` to return all the users in the database.

The User model (./app/models/user.rb)
```sh
class User < ApplicationRecord
end
```

Once the @users variable is defined, the controller calls the view (**Step 6**), shown in Listing below. Variables that start with the `@` sign, called instance variables, are automatically available in the views; in this case, the index.html.erb view iterates through the `@users` list and outputs a line of HTML for each one.

The view for the users index (./app/views/users/index.html.erb):
```erb
<p style="color: green"><%= notice %></p>

<h1>Users</h1>

<div id="users">
  <% @users.each do |user| %>
    <%= render user %>
    <p>
      <%= link_to "Show this user", user %>
    </p>
  <% end %>
</div>

<%= link_to "New user", new_user_path %>
```

The view converts its contents to HTML (**Step 7**), which is then returned by the controller to the browser for display (**Step 8**).


### 5.3 The Microposts resource

Having generated and explored the Users resource, we turn now to the associated Microposts resource. Throughout this section, I recommend comparing the elements of the Microposts resource with the analogous user elements from Section 5.2; you should see that the two resources parallel each other in many ways. The RESTful structure of Rails applications is best absorbed by this sort of repetition of form—indeed, seeing the parallel structure of Users and Microposts even at this early stage is one of the prime motivations for this chapter.

### 5.3.1 A micropost microtour

As with the Users resource, we’ll generate scaffold code for the Microposts resource using rails generate scaffold, in this case implementing the data model from Figure in Section 5.1.2.

```sh
$ rails generate scaffold Micropost content:text user_id:integer
      invoke  active_record
      create    db/migrate/<timestamp>_create_microposts.rb
      create    app/models/micropost.rb
      invoke    test_unit
      create      test/models/micropost_test.rb
      create      test/fixtures/microposts.yml
      invoke  resource_route
       route    resources :microposts
      invoke  scaffold_controller
      create    app/controllers/microposts_controller.rb
      invoke    erb
      create      app/views/microposts
      create      app/views/microposts/index.html.erb
      create      app/views/microposts/edit.html.erb
      create      app/views/microposts/show.html.erb
      create      app/views/microposts/new.html.erb
      create      app/views/microposts/_form.html.erb
      invoke    test_unit
      create      test/controllers/microposts_controller_test.rb
      create      test/system/microposts_test.rb
      invoke    helper
      create      app/helpers/microposts_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/microposts/index.json.jbuilder
      create      app/views/microposts/show.json.jbuilder
      create      app/views/microposts/_micropost.json.jbuilder
```

To update our database with the new data model, we need to run a migration as in Section 5.2:

```sh
$ rails db:migrate
==  CreateMicroposts: migrating ===============================================
-- create_table(:microposts)
   -> 0.0023s
==  CreateMicroposts: migrated (0.0026s) ======================================
```

Now we are in a position to create microposts in the same way we created users in Section 5.2.1. As you might guess, the scaffold generator has updated the Rails routes file with a rule for the Microposts resource, as seen in Listing below.

The Rails routes, with a new rule for the Microposts resource (./config/routes.rb):
```rb
Rails.application.routes.draw do
  resources :microposts
  resources :users
  root 'users#index'
end
```

As with users, the `resources :microposts` routing rule maps micropost URLs to actions in the Microposts controller, as seen in Table below.

| HTTP request method | URL                | Action    | Purpose                          |
|---------------------|--------------------|-----------|----------------------------------|
| GET                 | /microposts        | `index`   | page to list all microposts      |
| GET                 | /microposts/1      | `show`    | page to show micropost with id 1 |
| GET                 | /microposts/new    | new`      | page to make a new micropost     |
| POST                | /microposts        | `create`  | create a new micropost           |
| GET                 | /microposts/1/edit | `edit`    | page to edit micropost with id 1 |
| PATCH               | /microposts/1      | `update`  | update micropost with id 1       |
| DELETE              | /microposts/1      | `destroy` | delete micropost with id 1       |

The Microposts controller generated and note that, MicropostsController are so similar to the UsersController. This is a reflection of the REST architecture common to both resources.


To make some actual microposts, we click on New Micropost on the micropost index page (Figure below)
![image](./images/5.3.1_1.png)


and enter information at the new microposts page, `/microposts/new`, as seen in Figure below.
![image](./images/5.3.1_2.png)

At this point, go ahead and create a micropost or two, taking care to make sure that at least one has a `user_id` of `1` to match the `id` of the first user created before. The result should look something like Figure below
![image](./images/5.3.1_3.png)

#### 5.3.2. Putting the micro in microposts

Any micropost worthy of the name should have some means of enforcing the length of the post. Implementing this constraint in Rails is easy with validations; to accept microposts with at most 140 characters (à la the original design of Twitter), we use a length validation. At this point, you should open the file `app/models/micropost.rb` in your text editor or IDE and fill it with the contents of Listing below:
```rb
class Micropost < ApplicationRecord
  validates :content, length: { maximum: 140 }
end
```

As seen in Figure below, Rails renders error messages indicating that the micropost’s content is too long.

![image](./images/5.3.2.png)

#### 5.3.3. A user has_many microposts

One of the most powerful features of Rails is the ability to form **associations** between different data models. In the case of our User model, each user potentially has many microposts. We can express this in code by updating the User and Micropost models as in Listings below

A user has many microposts (`./app/models/user.rb`):
```rb
class User < ApplicationRecord
  has_many :microposts
end
```

A micropost belongs to a user (`./app/models/micropost.rb`):
```rb
class Micropost < ApplicationRecord
  belongs_to :user

  validates :content, length: { maximum: 140 }
end
```

We can visualize the result of this association in Figure below. Because of the `user_id` column in the `microposts` table, Rails (using Active Record) can infer the microposts associated with each user.

![image](./images/5.3.3.png)

For now, we can examine the implications of the user–micropost association by using the console, which is a useful tool for interacting with Rails applications. We first invoke the console with `rails console` at the command line, and then retrieve the first user from the database using `User.first` (putting the results in the variable `first_user`), as shown in Listing below (I include `exit` in the last line just to demonstrate how to exit the console. On most systems, you can also use `Ctrl-D` for the same purpose.)

```rb
$ rails console
>> first_user = User.first
User Load (0.1ms)  SELECT "users".* FROM "users" ORDER BY "users"."id"
ASC LIMIT ?  [["LIMIT", 1]]
 =>

>> first_user
 =>
#<User:0x00007ffbf1331658
 id: 1,
 name: "Michael Hartl",
 email: "michael@example.org ",
 created_at: Thu, 10 Mar 2022 00:23:31.441663000 UTC +00:00,
 updated_at: Thu, 10 Mar 2022 00:25:04.172206000 UTC +00:00>

#<User:0x00007ffbf1331658

>> first_user.microposts
  Micropost Load (0.2ms)  SELECT "microposts".* FROM "microposts"
  WHERE "microposts"."user_id" = ?  [["user_id", 1]]
 =>
[#<Micropost:0x00007ffbf16807a0
  id: 1,
  content: "First micropost!",
  user_id: 1,
  created_at: Thu, 10 Mar 2022 00:46:02.263125000 UTC +00:00,
  updated_at: Thu, 10 Mar 2022 00:46:02.263125000 UTC +00:00>,
 #<Micropost:0x00007ffbf1653a70
  id: 2,
  content: "Second micropost",
  user_id: 1,
  created_at: Thu, 10 Mar 2022 00:46:14.079131000 UTC +00:00,
  updated_at: Thu, 10 Mar 2022 00:46:14.079131000 UTC +00:00>]

>> micropost = first_user.microposts.first
 =>
#<Micropost:0x00007ffbf16807a0
...
>> micropost
 =>
#<Micropost:0x00007ffbf16807a0
 id: 1,
 content: "First micropost!",
 user_id: 1,
 created_at: Thu, 10 Mar 2022 00:46:02.263125000 UTC +00:00,
 updated_at: Thu, 10 Mar 2022 00:46:02.263125000 UTC +00:00>

>> micropost.user
 =>
#<User:0x00007ffbf1331658
 id: 1,post:0
 name: "Michael Hartl",
 email: "michael@example.org ",
 created_at: Thu, 10 Mar 2022 00:23:31.441663000 UTC +00:00,
 updated_at: Thu, 10 Mar 2022 00:25:04.172206000 UTC +00:00>

>> exit
```

The output includes the actual return values, which are raw Ruby objects, as well as the Structured Query Language (SQL) code that produced them.

In addition to retrieving the first user with `User.first`, the Listing shows two other things: (1) how to access the first user’s microposts using the code `first_user.microposts`, which automatically returns all the microposts with `user_id` equal to the `id` of `first_user` (in this case, **1**); and (2) how to return the user corresponding to a particular post using `micropost.user`.

#### 5.3.4. Inheritance hierarchies

Comparing Listings below, we see that both the User model and the Micropost model inherit (via the left angle bracket `<`) from `ApplicationRecord`, which in turn inherits from `ActiveRecord::Base`, which is the base class for models provided by Active Record; a diagram summarizing this relationship appears in Figure below. It is by inheriting from `ActiveRecord::Base` that our model objects gain the ability to communicate with the database, treat the database columns as Ruby attributes, and so on.


The `User` class (`./app/models/user.rb`):
```rb
class User < ApplicationRecord
  ...
end
```

The `Micropost` class (`./app/models/micropost.rb`):
```rb
class Micropost < ApplicationRecord
  ...
end
```

![image](./images/5.3.4_1.png)

The inheritance structure for controllers is essentially the same as that for models. Comparing Listing below, we see that both the _Users_ controller and the _Microposts_ controller inherit from the _Application_ controller. 

The `UsersController` class(`./app/controllers/users_controller.rb`):
```rb
class UsersController < ApplicationController
  ...
end
```

The `MicropostsController` class (`./app/controllers/microposts_controller.rb`):
```rb
class MicropostsController < ApplicationController
  ...
end
```


Examining Listing below, we see that ApplicationController itself inherits from ActionController::Base, which is the base class for controllers provided by the Rails library Action Pack. The relationships between these classes are illustrated in Figure below.

The ApplicationController class, highlighting inheritance (`./app/controllers/application_controller.rb`):
```rb
class ApplicationController < ActionController::Base
  ...
end
```

![image](./images/5.3.4_2.png)

As with model inheritance, both the _Users_ and _Microposts_ controllers gain a large amount of functionality by inheriting from a base class (in this case, `ActionController::Base`), including the ability to manipulate model objects, filter inbound HTTP requests, and render views as HTML. Since all Rails controllers inherit from `ApplicationController`, rules defined in the _Application_ controller automatically apply to every action in the application. 


