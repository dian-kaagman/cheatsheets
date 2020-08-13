# Rails

## Controller example
    class {Name}Controller < ApplicationController
       def index
           @properties = Property.all

            respond_to do |format|
                format.html
                format.json do
                    render json: @properties
                end
            end
       end
       
       # Error handling example in controller
       def update
           errors=[]
            begin 
                @instance.update(batch_params)
                errors += @instance.errors.to_a
            rescue Exception, StandardError => error
                errors.push(error)
                errors.push(*@instance.errors.full_messages) if @instance
            end

            errors.compact!
            if errors.empty?     
                render json: {data: @instance.to_json, status: 'ok'}
            else
                render json: {errors: errors, status: 'error'}
            end
       end
    end
    
    

## Routes example    
    Rails.application.routes.draw do
      root to: "home#index"

      get '/properties(.:format)',           controller: :{name},            action: :index
    end

## Import task
    namespace :import do

    desc "Import users from database"
    task :users => [:environment] do 
        Company.all.each do |company|
            puts company.email
            begin
                User.new(id: company.id, email: company.email, password: SecureRandom.hex).save! if company.email
            rescue => error
                puts error
                next
            end
        end
    end

    desc "Import products from json file"
      task :products => [:environment] do
                JSON.parse(File.read('/Volumes/TEMPORARY/DATA_EXCHANGE/retailers_shop/retailershop_products.json')).each{|product_attr|
              puts product_attr[:id]
              Product.find_or_create_by(product_attr).save!
          }
      end
    end

## Active Storage

Run command to install active storage

     rails active_storage:install
     rails db:migrate
     
Declare Active Storage services in `config/storage.yml`

## CORS

Install gem rack-cors

      gem install rack-cors

Add gem to gemfile

      gem 'rack-cors'
      
Add file cors.rb to /config/initializers
    
    Rails.application.config.middleware.insert_before 0, Rack::Cors do
        domains = ['example', 'example',' localhost:3000', '[::1]:3000', 'localhost:8080', '[::1]:8080']
        allow do
          origins *domains
          resource '*', headers: :any, methods: [:get, :post, :options, :delete, :put, :patch]
          # resource '/api/oauth/*', headers: :any, methods: [:get, :delete, :post, :options]
        end
    end

### Model
      class User < ApplicationRecord
        has_one_attached :picture
        
        def image
          self.picture.attachment ? Rails.application.routes.url_helpers.rails_blob_path(self.picture, only_path: true) :  "/images/no-image_available.png"
        end
      end
