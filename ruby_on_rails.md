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

### Model
      class User < ApplicationRecord
        has_one_attached :picture
        
        def image
          self.picture.attachment ? Rails.application.routes.url_helpers.rails_blob_path(self.picture, only_path: true) :  "/images/no-image_available.png"
        end
      end
