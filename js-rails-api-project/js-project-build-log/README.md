---
description: Keep a project journal and to-do list
---

# JavaScript Project Build Log

## General To Do List

* [x] **First thing:** Write up notes from what I did yesterday
* [x] Planning research:
  * [x] Polymorphic Associations
  * [x] Aliasing Associations
  * [x] Preloading Rails scopes via Associations
  * [x] Enums and Aliasing Associations
  * [x] Get in touch with Daniel about this
* [x] Map out my models and associations using an online tool

## Build Checklists

### Part 1: Planning and Building a Rails API

* [x] Generate a new Rails API app
* [x] Create a GitHub repo
* [x] Generate `User` model
  * [x] Add associations
  * [x] Add seed data
  * [x] Run migrations
* [x] Generate `TopicRequest` model
  * [x] Add associations
  * [x] Add seed data
  * [x] Run migrations
* [x] Create preset list of modules

### Part 2: Routes, Controllers, and Serializers

#### Routes

_\*Confirm routes with each new route implementation_

* [x] Implement `users#show` route
* [x] Implement `users#create` route
* [x] Implement `auth#create` route
* [x] Implement `auth#delete` route
* [x] Implement `topic_request#index` route
* [ ] Implement `topic_request#create` route
* [ ] Implement `topic_request#edit` route
* [ ] Implement `topic_request#delete` route

#### Controllers

* [x] Generate `User` controller
* [x] Generate `TopicRequest` controller

#### Serializers

* [x] Generate `User` serializer
  * [x] Add the `User` model attributes to the serializer
  * [x] Update the `User` controller actions to render the serialized data
* [x] Generate `TopicRequest` serializer
  * [x] Add the `TopicRequest` model attributes to the serializer
  * [x] Update the `TopicRequest` controller actions to render the serialized data
* [ ] Add model relationships either through the serializer or the controller

### Part 3: DOM Manipulation, Events, and Fetch Using Rails API

* [x] Create a separate frontend directory
* [x] Create an `index.html` file with an `index.js` script tag
* [x] Make sure your connection is established properly by testing it with a `console.log()`
* [x] Create another GitHub repo
* [x] Create an event listener for when the page is loaded
* [x] Set up Cross Origin Resource Sharing \(CORS\)
* [x] Create a `GET` fetch request
* [x] Create a `POST` fetch request
* [ ] DRY up your code

### Part 4: Object-Oriented JavaScript Refactor

* [x] Create a JavaScript class
  * [x] Should have a constructor function
  * [x] Should have an array containing all instances of the class
* [x] Create a static method to find a class object based on its `id`

