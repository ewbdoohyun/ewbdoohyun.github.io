---
title: Rails4 User Login - 처음부터
layout: post
comments: true
category: [dev, rails]
---	

- [Michael Hartl의 Rails tutorial][railstutorial] 에 나온 user login 기능 부분을 정리해 본다.
- User model 생성
	+ `rails generate model User name:string email:string`
	+ 
- Validation 추가
	{% highlight ruby %}
		class User < ApplicationRecord
		  before_save { email.downcase! }
		  validates :name, presence: true, length: { maximum: 50 }
		  VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
		  validates :email, presence: true, length: { maximum: 255 },
		                    format: { with: VALID_EMAIL_REGEX },
		                    uniqueness: { case_sensitive: false }
		end
	{% endhighlight %}
- Email에 대해 index 추가
	+ `rails generate migration add_index_to_users_email`
	+ {% highlight ruby %}
		class AddIndexToUsersEmail < ActiveRecord::Migration[5.0]
		  def change
		    add_index :users, :email, unique: true
		  end
		end
	{% endhighlight %}
- Password 추가(1)
	+ User 모델에 `has_secure_password` 추가
		+ method는 hashed password를 password_digest라는 attribute에 저장하도록 해준다.
		+ password 와 password_confirmation 두 virtual attributes를 사용하게 한다.
		+ `authenticate` method로  password가 맞는지 틀린지 알 수 있다.
    
    {% highlight ruby %}
        class User < ApplicationRecord
          .
          .
          .
          has_secure_password
        end
    {% endhighlight %}
- Password 추가(2)
    + password_digest attribute 추가
	   + `$ rails generate migration add_password_digest_to_users password_digest:string`
	+ `Gemfile` 에 bcrypt 추가
		+ `has_secure_password`에서 `bcrypt` hash function을 이용한다. 
		+ {% highlight ruby %}
			gem 'bcrypt',         '3.1.11'
		{% endhighlight %}
- Password validation 추가
	+ 최소 길이 6자
		* `validates :password, presence: true, length: { minimum: 6 }`
		* 
- Route.rb에 routing 추가
    + `resources :users`
- User Controller(1)
    + `rails generate controller Users`
    + `new` method
        + {% highlight ruby %}
            def new
                @user = User.new
            end
        {% endhighlight %}
- User Controller(2)
    + `new.html.erb` 에 form 추가
        * {% highlight ruby %}
        <%= form_for(@user) do |f| %>
          <%= f.label :name %>
          <%= f.text_field :name %>

          <%= f.label :email %>
          <%= f.email_field :email %>

          <%= f.label :password %>
          <%= f.password_field :password %>

          <%= f.label :password_confirmation, "Confirmation" %>
          <%= f.password_field :password_confirmation %>

          <%= f.submit "Create my account", class: "btn btn-primary" %>
        <% end %>
        {% endhighlight %}
- User Controller(3)
    +`strong parameter` 추가
        {% highlight ruby %}
         private

            def user_params
              params.require(:user).permit(:name, :email, :password,
                                           :password_confirmation)
            end
        {% endhighlight %}
- User Controller(4)
    + `create` method
        {% highlight ruby %}
          def create
            @user = User.new(user_params)
            if @user.save
              flash[:success] = "Welcome to the Sample App!"
              redirect_to @user
            else
              render 'new'
            end
          end
        {% endhighlight %}
- User Controller(5)
    + `new.html.erb` 에 error message 추가

        {% highlight ruby %}
        <%= form_for(@user) do |f| %>
            <%= render 'shared/error_messages' %>
        {% endhighlight %}

        {% highlight ruby %}
        #_error_messages.html.erb
        <% if @user.errors.any? %>
          <div id="error_explanation">
            <div class="alert alert-danger">
              The form contains <%= pluralize(@user.errors.count, "error") %>.
            </div>
            <ul>
            <% @user.errors.full_messages.each do |msg| %>
              <li><%= msg %></li>
            <% end %>
            </ul>
          </div>
        <% end %>
        {% endhighlight %}
- proudction 환경에서 SSL 추가
    + Heroku domain(appname.herokuapp.com)에서는 별도의 SSL setting이 필요없음.
    {% highlight ruby %}
        Rails.application.configure do
          # Force all access to the app over SSL, use Strict-Transport-Security,
          # and use secure cookies.
          config.force_ssl = true
        end
    {% endhighlight %}
- Session Controller(1)
    + ` rails generate controller Sessions new`
- Session Controller(2)
    - Router에 log-in 관련 routing 추가
        {% highlight ruby %}
            get    '/login',   to: 'sessions#new'
            post   '/login',   to: 'sessions#create'
            delete '/logout',  to: 'sessions#destroy'
        {% endhighlight %}
- Session Controller(3) 
    - `/sessions/new.html.erb` 에 log-in form 추가
        {% highlight ruby %}
        <%= form_for(:session, url: login_path) do |f| %>

          <%= f.label :email %>
          <%= f.email_field :email, class: 'form-control' %>

          <%= f.label :password %>
          <%= f.password_field :password, class: 'form-control' %>

          <%= f.submit "Log in", class: "btn btn-primary" %>
        <% end %>
        {% endhighlight %}
- Session Controller(4)
    - `create` method
        + `flash.now` 는 `flash`와 다르게 추가적인 reqeust가 있을 시 사라짐.
        {% highlight ruby %}
        def create
            user = User.find_by(email: params[:session][:email].downcase)
            if user && user.authenticate(params[:session][:password])
              # Log the user in and redirect to the user's show page.
            else
              flash.now[:danger] = 'Invalid email/password combination' # Not quite right!
              render 'new'
            end
          end
        {% endhighlight %}
- ApplicationController에 SessionHelper 추가 
    {% highlight ruby %}
    class ApplicationController < ActionController::Base
      protect_from_forgery with: :exception
      include SessionsHelper
    end
    {% endhighlight %}
- `app/helpers/sessions_helper.rb`
    {% highlight ruby %}
    module SessionsHelper

        # Logs in the given user.
        def log_in(user)
            session[:user_id] = user.id
        end
        # Returns the current logged-in user (if any).
        def current_user
            @current_user ||= User.find_by(id: session[:user_id])
        end
        def logged_in?
            !current_user.nil?
        end
          # Logs out the current user.
        def log_out
            session.delete(:user_id)
            @current_user = nil
        end
    end
    {% endhighlight %}
- Session Controller(5)
    + `create` method에 login 추가
    {% highlight ruby %}
    def create
        user = User.find_by(email: params[:session][:email].downcase)
        if user && user.authenticate(params[:session][:password])
          log_in user
          redirect_to user
        else
          flash.now[:danger] = 'Invalid email/password combination'
          render 'new'
        end
      end
    {% endhighlight %}
- Session Controller(6)
    + `destroy` method에 logout 추가
    {% highlight ruby %}
    def destroy
        log_out
        redirect_to root_url
    end
    {% endhighlight %}
    + 
[railstutorial]: https://www.railstutorial.org/book/_single-page
