# Devise
Add to gemfile `gem 'devise'`

      rails generate devise:install
      
Add to `config/environments/development.rb`  

      config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
      
      rails generate devise {ModelName}
      rails db:migrate

Add to controller

      before_action :authenticate_user!
      
Add to routes
  
      root to: '{controller}#{action}'
      
Helpers
  
      current_user
      user_signed_in
      user_session
