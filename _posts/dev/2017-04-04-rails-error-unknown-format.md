


ActionController::UnknownFormat




(before)

    resources :items do
        collection do 
          post :check
        end
      end


(after)

    resources :items do
        member do 
          post :check
        end
      end
