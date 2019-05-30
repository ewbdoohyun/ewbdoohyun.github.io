


/config/initializers/act_as_votable.md

    module ActsAsVotable
        class Vote < ::ActiveRecord::Base
            belongs_to :votable, :polymorphic => true, touch: true
        end
    end
