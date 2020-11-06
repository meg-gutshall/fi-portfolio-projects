# Part 2: Routes, Controllers, and Serializers

## Routes

* [ ] Implement _specific_ namespaced routes for _one_ controller at a time **for MVP**
  * [x] The API in our namespace tells the frontend that it will be fetching to a backend
* [x] Remember to version the routes as well

{% hint style="info" %}
**SEMANTIC VERSIONING**

Semantic versioning boils down to:

* **PATCH 0.0.x** level changes for implementation level detail changes, such as small bug fixes
* **MINOR 0.x.0** level changes for any backwards compatible API changes, such as new functionality/features
* **MAJOR x.0.0** level changes for backwards _incompatible_ API changes, such as changes that will break existing users' code if they update

Source: [RubyGems Guides](https://guides.rubygems.org/patterns/#semantic-versioning)
{% endhint %}

{% code title="config/routes.rb" %}
```ruby
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :users, only: [:index, :create]
      
      # Add in others as needed later
      resources :users
      resources :requests
      resources :upvotes
    end
  end
end
```
{% endcode %}

* [ ] Navigate to `http://localhost:3000/rails/info/routes` to confirm routes
  * [x] A more common option is to run `rails routes` in the terminal but I prefer the above method as it is cleaner and gives you more information
* [ ] Visit `http://localhost:3000/api/v1/users` to see error

## Controllers

* [ ] Generate a new controller:

{% tabs %}
{% tab title="User" %}
```bash
rails g controller api/v1/Users
```
{% endtab %}

{% tab title="Upvote" %}
```bash
rails g controller api/v1/Upvotes
```
{% endtab %}

{% tab title="Request" %}
```bash
rails g controller api/v1/Requests
```
{% endtab %}
{% endtabs %}

* [ ] When setting the `strong params` in each controller, we do not require the model object because we're building the new instances based on the input received from our frontend, which will not include the model object\*\*\*
* [ ] Upon successfully creating a new object, render it and send `status: :accepted`
* [ ] If there is an error in creating the new object, render the appropriate error and send `status: :unprocessible_entity`
  * [ ] These `status` attributes will return [HTTP status codes](https://httpstatuses.com/)

{% tabs %}
{% tab title="Users Controller" %}
{% code title="app/controllers/api/v1/users\_controller.rb" %}
```ruby
class Api::V1::UsersController < ApplicationController
  def index
    users = User.all
    render json: users
  end
  
  def create
    user = User.new(user_params)
    if user.save
      render json: user, status: :accepted
    else
      render json: { errors: user.errors.full_messages }, status: :unprocessible_entity
    end
  end
  
  private
    def user_params
      params.permit(:first_name, :last_name, :email_address, :password_digest, :role)
    end
end
```
{% endcode %}
{% endtab %}

{% tab title="Upvotes Controller" %}
{% code title="app/controllers/api/v1/upvotes\_controller.rb" %}
```ruby
class Api::V1::UpvotesController < ApplicationController
  def index
    upvotes = Upvote.all
    render json: upvotes
  end
  
  def create
    upvote = Upvote.new(user_params)
    if upvote.save
      render json: upvote, status: :accepted
    else
      render json: { errors: upvote.errors.full_messages }, status: :unprocessible_entity
    end
  end
  
  private
    def upvote_params
      params.permit(:comment, :student_id, :request_id)
    end
end
```
{% endcode %}
{% endtab %}

{% tab title="Requests Controller" %}
{% code title="app/controllers/api/v1/requests\_controller.rb" %}
```ruby
class Api::V1::RequestsController < ApplicationController
  def index
    requests = Request.all
    render json: request
  end
  
  def create
    request = Request.new(user_params)
    if request.save
      render json: request, status: :accepted
    else
      render json: { errors: request.errors.full_messages }, status: :unprocessible_entity
    end
  end
  
  private
    def request_params
      params.permit(:topic, :module, :description, :student_id)
    end
end
```
{% endcode %}
{% endtab %}
{% endtabs %}

* [ ] Make sure you have the relevant routes you need for you MVP
  * [ ] If not, [build them!](part-2-routes-controllers-and-serializers.md#routes)
* [ ] Visit `http://localhost:3000/api/v1/<your_resource_name>` to see your seed data rendered in the browser in JSON format

## Serializers

* [ ] Add the gem `fast_jsonapi` to your project by running `bundle add fast_jsonapi`
  * [ ] Gem has a configuration that will automatically convert camel case to snake case and back
* [ ] Generate serializer classes:

{% tabs %}
{% tab title="User" %}
```bash
rails g serializer User
```
{% endtab %}

{% tab title="Upvote" %}
```bash
rails g serializer Upvote
```
{% endtab %}

{% tab title="Request" %}
```bash
rails g serializer Request
```
{% endtab %}
{% endtabs %}

* [ ] Add the model attributes to the newly generated serializer

{% tabs %}
{% tab title="User Serializer" %}
{% code title="app/serializers/user\_serializer.rb" %}
```ruby
class UserSerializer
  include FastJsonapi::ObjectSerializer
  attributes :first_name, :last_name, :email_address, :role
end
```
{% endcode %}
{% endtab %}

{% tab title="Upvote Serializer" %}
{% code title="app/serializers/upvote\_serializer.rb" %}
```ruby
class UpvoteSerializer
  include FastJsonapi::ObjectSerializer
  attributes :comment, :request_id, :request, :student_id, :student
end
```
{% endcode %}
{% endtab %}

{% tab title="Request Serializer" %}
{% code title="app/serializers/request\_serializer.rb" %}
```ruby
class RequestSerializer
  include FastJsonapi::ObjectSerializer
  attributes :topic, :module, :description, :student_id, :student
end
```
{% endcode %}
{% endtab %}
{% endtabs %}

* [ ] Update the model's controller actions to render the serialized data

{% tabs %}
{% tab title="Users Controller" %}
{% code title="app/controllers/api/v1/users\_controller.rb" %}
```ruby
class Api::V1::UsersController < ApplicationController
  def index
    users = User.all
    render json: UserSerializer.new(users)
  end
  
  def create
    user = User.new(user_params)
    if user.save
      render json: UserSerializer.new(user), status: :accepted
    else
      render json: { errors: user.errors.full_messages }, status: :unprocessible_entity
    end
  end
  
  private
    def user_params
      params.permit(:first_name, :last_name, :email_address, :password_digest)
    end
end
```
{% endcode %}
{% endtab %}

{% tab title="Upvotes Controller" %}
{% code title="app/controllers/api/v1/upvotes\_controller.rb" %}
```ruby
class Api::V1::UpvotesController < ApplicationController
  def index
    upvotes = Upvote.all
    render json: UpvoteSerializer.new(upvotes)
  end
  
  def create
    upvote = Upvote.new(user_params)
    if upvote.save
      render json: UpvoteSerializer.new(upvote), status: :accepted
    else
      render json: { errors: upvote.errors.full_messages }, status: :unprocessible_entity
    end
  end
  
  private
    def upvote_params
      params.permit(:comment, :student_id, :request_id)
    end
end
```
{% endcode %}
{% endtab %}

{% tab title="Requests Controller" %}
{% code title="app/controllers/api/v1/requests\_controller.rb" %}
```ruby
class Api::V1::RequestsController < ApplicationController
  def index
    requests = Request.all
    render json: RequestSerializer.new(requests)
  end
  
  def create
    request = Request.new(user_params)
    if request.save
      render json: RequestSerializer.new(request), status: :accepted
    else
      render json: { errors: request.errors.full_messages }, status: :unprocessible_entity
    end
  end
  
  private
    def request_params
      params.permit(:topic, :module, :description, :student_id)
    end
end
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/Netflix/fast\_jsonapi" %}

{% hint style="info" %}
**NOTE:** Use this version of the gem instead `gem 'fast_jsonapi', '~> 1.7.1', git: 'https://github.com/fast-jsonapi/fast_jsonapi'` as it's kept more up-to-date
{% endhint %}

* [ ] When adding our model relationships to our rendered JSON output via the `fast_jsonapi` gem, we have two options: add the Active Record association on the serializer or add the relationship as an option in the controller

{% tabs %}
{% tab title="Relationships via Serializer" %}
{% code title="app/serializers/request\_serializer.rb" %}
```ruby
class RequestSerializer
  include FastJsonapi::ObjectSerializer
  set_id :student_id
  attributes :topic, :module, :description
  has_many :upvotes
  belongs_to :student, serializer: :user
end
```
{% endcode %}
{% endtab %}

{% tab title="Relationships via Controller" %}
{% code title="app/controllers/api/v1/requests\_controller.rb" %}
```ruby
class Api::V1::RequestsController < ApplicationController
  def index
    requests = Request.all
    options = {
      include: [:student, :upvotes]
    }
    render json: RequestSerializer.new(requests)
  end
  
  def create
    request = Request.new(user_params)
    if request.save
      render json: RequestSerializer.new(request), status: :accepted
    else
      render json: { errors: request.errors.full_messages }, status: :unprocessible_entity
    end
  end
  
  private
    def request_params
      params.permit(:topic, :module, :description, :student_id)
    end
end
```
{% endcode %}
{% endtab %}
{% endtabs %}

* [ ] **TEST:** Confirm that your seed data renders properly by visiting `http://localhost:3000/api/v1/<your_resource_name>`

{% hint style="warning" %}
**NOTE:** Experiment with your serializers and the resulting output formats. There are TONS of options! Be prepared to come back to your serializers and adjust them throughout the project. To quote Ayana Cotton Zaire, "They are very nesty!"
{% endhint %}

## Sources

### Notes

{% embed url="https://github.com/AyanaZaire/seeda\_syllabus\_backend/blob/master/project-build-part2-notes.md" caption="Project Build Part 1 Review & Project Build Part 2 Notes" %}

### Video

{% embed url="https://youtu.be/sLrFiwhMPZU" caption="Project Build Part 2 Video" %}

