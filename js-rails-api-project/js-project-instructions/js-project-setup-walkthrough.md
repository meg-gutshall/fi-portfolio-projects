---
description: As extracted from the JavaScript Project Instructions GitHub repository
---

# JavaScript Project Setup Walkthrough

{% embed url="https://github.com/learn-co-students/js-spa-project-instructions-online-web-sp-000/blob/master/setup-walkthrough.md" caption="setup-walkthrough.md" %}

For your project, you will be building a Ruby on Rails backend and JavaScript frontend. Two parts of the application talking to each other means there will be a lot of moving parts! This document will walk you through setting up your project.

Since you want to keep you frontend and backend code separate, you will have two separate top-level folders in your project, one for the Rails app and one for the JavaScript app. When you finish this walkthrough, your file structure will look like:

```bash
javascript-project/
  backend/
    app/
    (...other Rails files and folders)
  frontend/
    index.html
    style.css
    index.js
  README.md
```

## Creating the Top-level Folder

Once you have planned your project, choose a location on your computer where you'd like to save your project. Move to that location in the terminal with the `cd` command. For this walkthrough, we'll call the project 'walkthrough-project'. You should choose a name that fits your project.

```bash
mkdir walkthrough-project
cd walkthrough-project
```

## Setting Up the Backend Rails API

When you create a news Rails application, Rails provides you with a ton of stuff that you will not need in order to build an API. Think of the entire `ActionView` library \(all the view helper methods like `form_for`\), sessions and cookie-handling, etc.

[Rails provides a way to set up a project for an API.](https://guides.rubyonrails.org/api_app.html) It will include what you need for an API, and exclude the unnecessary extras. Recall the lesson on [Creating a Rails API from Scratch](https://learn.co/tracks/full-stack-web-development-v8/module-14-front-end-web-programming-in-javascript/section-6-rails-as-an-api/creating-a-rails-api-from-scratch).

### Create a New Rails API Project

In you terminal, enter:

```bash
rails new walkthrough-backend --api --database=postgresql
```

For your project, `rails new <my_app_name> --api`.

> _Replace `<my_app_name>` with the actual name of your project._

This will generate a new Rails project using PostgreSQL as the database. We specify the `--api` flag so Rails knows to set this up as an API.

The `config.api_only = true` option found in `config/application.rb` tells Rails that our app will be an **API only**. In other words, our API **will not generate any HTML**, instead, it will return JSON. The frontend is responsible for fetching the data as JSON and formatting it to show to the user.

{% hint style="info" %}
[Read here](https://www.w3schools.com/js/js_json_intro.asp) if you want to review what JSON is and why we use it.
{% endhint %}

### Configure CORS

> [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a security feature that only allows API calls from known origins. For example, if someone tried to use some malicious JavaScript to steal you bank information and your bank allowed API calls from anywhere, this could be a bad situation.
>
> Let's say your bank is hosted on `https://bankofamerica.com`. If a clever hacker tried to impersonate you and send a request with an _origin_ of `https://couponvirus.org`, then ideally, your bank would reject requests from unknown _origins_ such as `couponvirus.org` and only allow requests from trusted origins or domains like `bankofamerica.com`.
>
> When developing your application, the backend and frontend may be served from different origins. Turning on CORS in your backend will allow your frontend to make requests, even though it's on a different origin.

* `cd` into the new project folder you just created
* Navigate to your `Gemfile` and uncomment `gem 'rack-cors'`
  * This will allow us to set up Cross Origin Resource Sharing \(CORS\) in our API
* Run `bundle install`
* Inside of `config/initializers/cors.rb` uncomment the following code:

```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'
    
    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

> This snippet is from [the documentation for the rack-cors gem](https://github.com/cyu/rack-cors).

Inside the `allow` block, `origins '*'` means we are allowing requests from **all** origins and are allowing `[:get, :post, :put, :patch, :delete, :options, :head]` requests to the API.

{% hint style="info" %}
[Read here](https://www.w3schools.com/tags/ref_httpmethods.asp) if you need a refresher on the HTTP request methods.
{% endhint %}

For now, leave the origins open. Later on, you can change this to only allow requests from the address of the frontend repo — `localhost:8000` or `www.myapp.com` for example.

### Models, Seeds, Routes, and Controller

Now that you have the initial Rails app configured, you should add your first model, routes, controller, and seed data. You should test you code along the way using the Rails console \(`rails c`\). When you have you controller and routes set up, you can see them in action by running the Rails server \(`rails s`\) and opening your app in the browser.

{% hint style="warning" %}
Make sure you have run your migrations and that PostgreSQL is running whenever you start up your Rails server!
{% endhint %}

## Setting Up the Frontend \(Client\) Application

In Rails, the framework is _very opinionated_. That is, it has a lot of opinions about how you code should be structured — which files and which methods should go where. The same rules don't apply for your JavaScript app. There is no "one right way" to structure your code. In this project, we are not using a frontend framework and the design decisions will be up to you.

The walkthrough will demonstrate one reasonable way to structure the application. You can structure your code in a similar pattern, but should not feel tied to exactly these files or organization.

### Initial Setup

Make a new directory for your frontend in your top-level 'walkthrough-project' folder and `cd` into it.

```bash
mkdir walkthrough-frontend
cd walkthrough-frontend
```

{% hint style="info" %}
**TIP:** You can open up a new tab in terminal with `Cmd + t` if you'd like to have your Rails server up and running in another tab.
{% endhint %}

In the new folder, create a single HTML page for your application, and a folder to hold your JavaScript files.

```bash
touch index.html
mkdir src
```

Start with a single JavaScript file. Later on, you can split your code into multiple files for organization.

```bash
touch src/index.js
```

In `index.html`, you need to add some HTML. Text editors will often have a shortcut for creating a blank HTML document.

```markup
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    Sample text - replace me!
  </body>
</html>
```

Open this file in your browser to make sure things are working.

To get the JavaScript part of the project up and running, link the JavaScript file to your HTML page with a `<script>` tag:

```markup
<script type="application/javascript" src="src/index.js" charset="utf-8"></script>
```

To check that your JavaScript file is linked correctly, add a log statement, refresh, and view the result in the JavaScript console.

```javascript
console.log("testing...");
```

## Connecting Rails and JavaScript: Make a Request with `fetch()`

Now that you have a Rails server running and have an HTML page with some JavaScript executing, you need to make sure that they can talk to each other.

If you haven't set up a controller yet on the Rails side, you can use this test code to make sure that things are working correctly. If you've set up a controller, feel free to test with that instead!

{% tabs %}
{% tab title="Controller" %}
{% code title="walkthrough-backend/app/controllers/application\_controller.rb" %}
```ruby
class ApplicationController < ActionController::API
  def test
    render json: { test: "success" }
  end
end
```
{% endcode %}
{% endtab %}

{% tab title="Routes" %}
{% code title="walkthrough-backend/config/routes.rb" %}
```ruby
Rails.application.routes.draw do
  get '/test', to: 'application#test'
end
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="walkthrough-frontend/src/index.js" %}
```javascript
// Test that we can fetch data from the backend
const BACKEND_URL = 'localhost:3000';
fetch(`${BACKEND_URL}/test`)
  .then(response => response.json())
  .then(parsedResponse => console.log(parsedResponse));
```
{% endcode %}
{% endtab %}
{% endtabs %}

If you have your Rails server running, and you refresh your HTML page, you should see the success message logged to the JavaScript console.

## Building the Rest of Your Application

Okay, so you've gotten this far. You have a Rails app running and you have a vanilla HTML and JavaScript client app that can talk to the Rails app. You'll want to add features to the project to match your design. Where to next?

Some things to consider:

* To add _nouns_ to your app, create models in Rails
* To _show_ your nouns, you'll need:
  * A controller action to send the data
  * A fetch request to ask for the data
  * Some JavaScript code to handle DOM-rendering
* To make your page respond to the user, you'll need _event listeners_
* To keep your code clean, you should use JavaScript _classes_
* To organize your code, you can use multiple JavaScript files, but don't forget to add `<script>` tags for each one

Good luck, and happy building!

## Source

{% embed url="https://github.com/learn-co-students/js-spa-project-instructions-online-web-sp-000/blob/master/setup-walkthrough.md" caption="JavaScript Project Setup Walkthrough" %}

