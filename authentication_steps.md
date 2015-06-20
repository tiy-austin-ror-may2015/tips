##Authentication steps

A guide to implement user authentication in your rails app.

###You will be working with the following files:

* Application Controller (app > controllers > application_controller.rb)
* Routes (config > routes.rb)
* A new file called Sessions Controller (app > controllers > sessions_controller.rb)
* The User model (app > models > user.rb)
* A new file called new.html.rb in a new folder called sessions under the views folder (app > views > sessions > new.html.erb)

###Before you start!
Before we start setting up our user validation, you need to check two things:

* Make sure your User table has "password_digest" in it to store your passwords securely.

* Make sure your User model includes 'has_secure_password' in the model.

* Go to your Users Controller and make sure you have "password" and "password_confirmation" as part of the user params (typically these are under 'private'), like this:
```
def user_params
  params.require(:user).permit(:username, :email, :password, :password_confirmation)
end
```

###Step by step user validation

* Go to your Application Controller file and add the following "current_user" method and the "authenticate_user!" method below. Also make sure you change `"protect_from_forgery"` to `"with: :exception"`.

```
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  before_filter :current_user

  def current_user
    if @current_user.nil?
      if session[:user_id].present?
        @current_user = User.find(session[:user_id])
      end
    else
      @current_user
    end
  end

  def authenticate_user!
    unless current_user
      flash[:alert] = "You must be logged in to do that!"
      redirect_to login_path
    end
  end

end
```

* You will now go to your routes file and add the routes necessary to route the user to login and logout. This will send the user to the correct action to create and destroy a session, which we will be creating in the next step. The routes to add to your routes.rb file are:

```
get 'login', to: 'sessions#new',    as: 'login'
post 'login', to: 'sessions#create', as: 'create_session'
get 'logout', to: 'sessions#destroy', as: 'logout'
```

* Now we need to create a sessions controller to handle all of the actions related to a user session. Navigate to `app > controllers` and add a brand new file called "sessions_controller.rb" that inherts from our applicaton controller, like this:

```
class SessionsController < ApplicationController

end
```

* In the new sessions controller file, you will need to add the new, create and destroy actions which will — you guessed it — create and destroy user sessions.

```
class SessionsController < ApplicationController
  def new
  end

  def create
    user = User.find_by_email(params[:email])
    if user && user.authenticate(params[:password])
      session[:user_id] = user.id
      redirect_to root_url, notice: 'You logged in successfully! Hooray!'
    else
      flash[:alert] = "Username or email did not match."
      render :new
    end
  end

  def destroy
    session[:user_id] = nil
    redirect_to root_url, notice: 'Successfully logged out.'
  end

end
```

**Please note:** You can redirect users to the root url (or any other url you want), or you can also render JSON.

* Now that we have our routes and actions for logging in, we will need a view for our users to actually login. Go to `app > views` and create a new folder called "sessions".

* In the new "sessions" folder you just made in the views folder, create a new file called "new.html.erb". This will be our form to login.

* Add a form in your "new.html.erb" that allows users to login. A sample form is below:

```
<div class="container">
  <h2>Login</h2>

  <%= form_tag create_session_path do %>
    <p>Email: <%= text_field_tag :email %></p>
    <p>Password: <%= password_field_tag :password %></p>
    <%= button_to("Login", {action: "create"}, class: "btn btn-primary") %>

  <% end %>
</div>
```
* Now, we need to build a helper model to help us check if a user is logged in. Go to `app > helpers > application_helper.rb` and add the following 'logged_in?' method:

```
module ApplicationHelper
  def logged_in?
    unless @current_user.nil?
      true
    end
  end
end
```

####Now what?
Now that you have setup your user authentication, all you have to do is call the "authenticate_user!" method on the actions that should be behind an authentication wall.

For example, if only logged in users can write posts, then you would call "authenticate_user!" on your Posts Controller under your "create" action.

Because "authenticate_user!" is in the Application Controller, and our controllers inherit from Application Controller — you guessed it — we have access to it!

Boom, you just implemented user validation in your app.

