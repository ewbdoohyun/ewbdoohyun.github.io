---
title: Gem - Factory_girl
layout: post
comments: true
category: [dev, rails, test]
--- 



<small>FactoryGirl Rails v4.4 기준입니다.</small>

<br>

### Fixture 쓰지 말고, FactoryGirl 쓰자.

- Fixture는 Active Record를 우회해서 validation이 안된다.
- 데이터가 쉽게 변경되어서 test code만큼 test db 관리에 신경써야 된다.

<br>

### Valid / invalid factory

한 모델에 대해 factory를 만들 때 기본적으로 valid / invalid factory를 생성해둔다.

    # app/models/item.rb
    class Item < ActiveRecord::Base
      validates :name, presence: true
      ...
    end


    # spec/factories.rb
    factory :item do
      sequence(:name) { |n| "Item_#{n}"}
      ...

      factory :invalid_item do
        name nil
      end
    end

valid/invalid factory는 `attributes_for` method를 이용해서 controller spec에서 사용한다.

    # spec/controllers/item_controller_spec.rb
    context "with valid attributes" do
      it "saves the new item into the database" do
        expect {
          post :create, item: attributes_for(:item)
          }.to change(Item, :count).by(1)
      end
    end
    context "with invalid attributes" do
      it "does not save the new item into the database" do
        expect {
          post :create, item: attributes_for(:invalid_item)
        }.not_to change(Item, :count)
      end
    end

<br>

### Association

nested_attribute를 설정한 경우, factory에도 nested attribute를 받아서 assocation factory를 생성하게 할 수 있다.

    # app/models/item.rb
    class Item < ActiveRecord::Base
      ...
      accepts_nested_attributes_for :keywords, allow_destroy: true
      ...

      #strong parameter
      
      def item_params
        params.require(:item).permit(:id, :name, keywords_attributes: [:id, :name, '_destroy')
      end

associated factory에 넘길 parameter와 생성할 assocated factory 갯수를 `ignore`로 설정해준다.

    # spec/factories.rb

    factory :item do
      sequence(:name) { |n| "Item_#{n}"}

      factory :item_with_keywords do
        ignore do
          keywords_count 1
          nmc 0
        end
        after(:create) do |item, evaluator|
          create_list(:keyword, evaluator.keywords_count, {item: item, nmc: evaluator.nmc})
        end
      end
    end

    factory :keyword do
        sequence(:name) { |n| "Keyword_#{n}"}
      end

factory 생성시에 strong parameter와 같이 nested attributes를 함께 `create` method에 넘겨준다.

    item = create(:item, keywords_attributes: [attributes_for(:keyword)] )

    post :create, item: attributes_for(:item, keywords_attributes: [attributes_for(:keyword)]))

<br>

### Faker Gem

[faker][3] 를 이용하면 이름/이메일/주소 등 실제 데이터같은 샘플 데이터를 얻을 수 있다.

    # Gemfile
    group :development, :test do
      gem 'faker', '~> 1.4.3'
    end

    # spec/factories.rb
    factory :user do
      name Faker::Name.name
    end

### 내가 겪은 Trouble

`has_secure_password` 를 사용했을 때, factory_girl로 만든 `user`로 login하는데 문제가 있었다. 원인은 아직모르겠으나, factory를 사용하지 않고, ActiveRecord를 직접 생성하는 것으로 우회했다.

    # Not working
    factory :user do
      factory :admin do
      end
    end

    # Workaround
    factory :user do
    end

    factory :admin, class: User do
    end

<br>

---

[Model과 다른 이름의 factory 만드는 guide][1]
[FactoryGirl Cheat Sheet][2]

[1]: https://github.com/thoughtbot/factory_girl/blob/master/GETTING_STARTED.md#defining-factories
[2]: http://ricostacruz.com/cheatsheets/factory_girl.html
[3]:https://github.com/stympy/faker
