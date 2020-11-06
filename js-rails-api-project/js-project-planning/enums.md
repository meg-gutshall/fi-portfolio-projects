---
description: An overview on how enums work
---

# Enums

## Enum Roles

{% tabs %}
{% tab title="User Model" %}
{% code title="app/models/user.rb" %}
```ruby
class User < ApplicationRecord
  enum role: [:student, :instructor, :super_admin]
  
  # Other user model code goes here...
end
```
{% endcode %}
{% endtab %}

{% tab title="Migration Generator Code" %}
```bash
rails g migration AddRoleToUsers role
```
{% endtab %}

{% tab title="Migration File" %}
{% code title="app/db/migrate/date\_add\_role\_to\_users.rb" %}
```ruby
class AddRoleToUsers < ActiveRecord::Migration
  def change
    add_column :users, :role, :integer, default: 0
  end
end
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Available Methods & Queries

Per the code blocks above, we have now declared an enum attribute \(`role`\) where the values map to integers in the database but can be queried by name \(`student`, `instructor`, or `super_admin`\). Like other attributes, we can hard code its value, however, it has to be one of the enum field's declared values.

```ruby
user.role = nil
user.role.nil?    # => true
user.role         # => nil

# user.role = 0
user.role = "student" # This is equivalent to line 5
user.role             # => "student"
```

Additionally, Rails creates boolean \(`?`\) and dangerous \(`!`\) methods for each value, or in this case, accessor and update methods. Boolean methods \(also called predicates or query methods\) end with a question mark \(`?`\) and return `true` or `false`. Dangerous methods \(also called mutator or bang methods\) end with the bang operator \(`!`\) and return the modified object.

```ruby
# user.update! role: 1
user.instructor! # This is equivalent to line 9
user.instructor? # => true
user.role        # => "instructor"

user.super_admin? # => false
user.super_admin! # This line is changing the user role from instructor to super_admin
user.super_admin? # => true
```

Scopes based on the allowed values of the enum field are provided as well. Remember, you can always query them directly using ActiveRecord.

{% tabs %}
{% tab title="Scopes" %}
```ruby
User.roles # => { "student" => 0, "instructor" => 1, "super_admin" => 2 }

User.student     # Returns all student users
User.not_student # Returns all instructor and super_admin users

User.instructor     # Returns all instructor users
User.not_instructor # Returns all student and super_admin users

User.super_admin     # Returns all super_admin users
User.not_super_admin # Returns all student and instructor users
```
{% endtab %}

{% tab title="ActiveRecord Queries" %}
```ruby
# All queries return an ActiveRecord::Relation object
User.where(role: :student)     # Returns all student users
User.where(role: :instructor)  # Returns all instructor users
User.where(role: :super_admin) # Returns all super_admin users

# Returns all instructor and super_admin users
User.where.not(role: :student)
User.where(role: [:instructor, :super_admin])

# Returns all student and super_admin users
User.where.not(role: :instructor)
User.where(role: [:student, :super_admin])

# Returns all student and instructor users
User.where.not(role: :super_admin)
User.where(role: [:student, :instructor])
```
{% endtab %}
{% endtabs %}

### Resources

{% embed url="https://api.rubyonrails.org/classes/ActiveRecord/Enum.html" %}

