---
layout: post
title: "Namespacing to Add New Actors"
date: 2013-08-11 17:00
comments: true
categories: 
---
Namespacing can be used to add a new **actor** to an application - an
Admin, who has special access, for instance. Let's say we want to have a
Users Controller that is accessible only by admins. To do this, we route
requests to a Users Controller through the Admin namespace (generating a
route of */admin/users/*) in config/routes.rb:
```ruby
namespace :admin do   
  resources :users 
end
```
Rails generates a path of`/admin/users` and a helper of admin\_users, a
path of `/admin/users/new` and a helper of new\_admin\_user, etc. And
now we need a new **Users** controller that we place in the
`app/controllers/admin` directory.

### Solution 1 - Regular Namespaced Controller

The simplest way to add in the new Admin actor is to create a new
`users_controller.rb` in that directory, have it inherit from the
Application Controller like a regular controller, and include an
appropriate before\_filter defined in the ApplicationController that
will ensure that a user accessing any actions from that controller is an
admin:
```ruby
class Admin::UsersController < ApplicationController  
  before_filter :ensure_admin
```
However, this is not the most efficient solution for larger apps where
many controllers will be delegated to the Admin namespace - every
controller would need to include the `before_filter :ensure_admin` line,
which is not very DRY.

### Solution 2 - Inheriting from an AdminsController

A more efficient way to do it is by having each namespaced class inherit
from a separate Admin class, which itself inherits from
ApplicationController and includes the before\_filter. That way every
inheriting subclass will require admin authorization:
<span style="text-decoration:underline;">app/controllers/admin/users\_controller.rb</span>
` class Admin::UsersController < AdminsController`   def create   end
... end
<span style="text-decoration:underline;">app/controllers/admins\_controller.rb</span>
``` ruby
class AdminsController < ApplicationController   
  before_filter :ensure_admin   

  def ensure_admin   ....   
  end 
end
```
Namespacing can be used to add any actor with particular access - a
merchant, customer, admin, guest, etc. And actors can inherit from each
other in order to share privileges as well. Now if you want to submit a
model-backed form to a namespaced model, the syntax is similar to nested
resources: ` form_for [:admin, @user] do ...`