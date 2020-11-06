---
description: Features beyond the scope of the minimum viable product (MVP)
---

# Future Features

## Build Structure

### Model Attributes & Migrations

{% tabs %}
{% tab title="User" %}
{% code title="app/db/migrate/date\_create\_users.rb" %}
```ruby
class CreateUsers < ActiveRecord::Migration[6.0]
  def change
    create_table :users do |t|
      t.string  :first_name
      t.string  :last_name
      t.string  :email_address
      t.string  :password_digest
      t.integer :role
      
      t.timestamps null: false
    end
  end
end
```
{% endcode %}
{% endtab %}

{% tab title="Upvote" %}
{% code title="app/db/migrate/date\_create\_upvotes.rb" %}
```ruby
class CreateUpvotes < ActiveRecord::Migration[6.0]
  def change
    create_table :upvotes do |t|
      t.text :comment
      t.references :codepanion
      t.references :topic_request
      
      t.timestamps null: false
    end
  end
end
```
{% endcode %}
{% endtab %}

{% tab title="TopicRequest" %}
{% code title="app/db/migrate/date\_create\_topic\_requests.rb" %}
```ruby
class CreateTopicRequests < ActiveRecord::Migration[6.0]
  def change
    create_table :topic_requests do |t|
      t.string     :topic
      t.text       :description
      t.string     :category
      t.references :codepanion
      
      t.timestamps null: false
    end
  end
end
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Model Associations

Two types:

#### `User` — `TopicRequest`

The associations for when a `User` creates a new `TopicRequest`

#### `User` — `Upvote` — `TopicRequest`

The associations for when a `User` creates an `Upvote` on an existing `TopicRequest`

{% tabs %}
{% tab title="User" %}
{% code title="app/models/user.rb" %}
```ruby
class User < ApplicationRecord
  has_many :topic_requests, foreign_key: :codepanion_id
  
  has_many :upvotes, foreign_key: :codepanion_id
  has_many :upvoted_requests, through: :upvotes, source: :topic_request
  
  enum role: {codepanion: 0, super_admin: 1}
end
```
{% endcode %}
{% endtab %}

{% tab title="Upvote" %}
{% code title="app/models/upvote.rb" %}
```ruby
class Upvote < ApplicationRecord
  belongs_to :codepanion, class_name: "User"
  belongs_to :topic_request
end
```
{% endcode %}
{% endtab %}

{% tab title="TopicRequest" %}
{% code title="app/models/topic\_request.rb" %}
```ruby
class TopicRequest < ApplicationRecord
  belongs_to :codepanion, class_name: "User"
  
  has_many :upvotes
  has_many :supporting_codepanions, through: :upvotes, source: :codepanions
end
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Domain Model

![](../../.gitbook/assets/model-map-codepanion-users.png)

## Functioning

### Authorization

* `Users` cannot create `Upvotes` against their own `TopicRequests`
* Possible delete and edit rules:
  * **Edit**: A notification goes out to all users who upvoted the `TopicRequest` \(?\)
  * **Delete**: `User` can delete their `TopicRequest` and what happens next depends on the amount of `Upvotes`
    * Popular `TopicRequests` get transferred to a service account
    * Unpopular `TopicRequests` get deleted

### Add GitHub OAuth

Use `username` instead of `email` for login credentials

{% code title="app/models/user.rb" %}
```ruby
# Code for GitHub OAuth username credentials
request.env["omniauth.auth"]["extra"]["raw_info"]["login"]
```
{% endcode %}

## Display

### `TopicRequests` Index

* Create a search box
* Create a sorting function
* Create a filter function

### Code Talk Sessions

* Use more as a tool to lay on top of the Code Talks website
* Create two popularity sections

