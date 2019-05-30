

https://railskey.wordpress.com/2012/09/07/rails-3-how-to-redirect_to-in-ajax-call/

http://stackoverflow.com/questions/29310187/rails-invalidcrossoriginrequest

     class CategoriesController < ApplicationController
       before_action :check_for_mobile, only: [:show, :update_menus]
       protect_from_forgery except: :show

    
        respond_to do |format|
          format.html {
              redirect_to login_path
          }
          format.js {
            render :js => "window.location.pathname = '#{login_path}'"
          }

