---
title: Rails - has_and_belongs_to_many
layout: post
comments: true
category: [dev, rails]
--- 



## Option

[distinct][2]
중복 assocation 생성을 금지

    class Ingredient < ActiveRecord::Base
        has_and_belongs_to_many :items, -> {distinct}

ingredient.items.size       // 1
ingredient.items << new_item
ingredient.items << new_item
ingredient.items            // 2



## Method

[collection.delete(object)][1]
- assocation관계만 제거. object가 제거되지는 않음

collection.delete_all
- 모든 association관계 제거. object는 제거되지 않음

    irb(main):003:0> Item.find(139).ingredients.delete_all
    Item Load (0.8ms)  SELECT  "items".* FROM "items" WHERE "items"."id" = $1 LIMIT 1  [["id", 139]]
    Ingredient Load (1.1ms)  SELECT DISTINCT "ingredients".* FROM "ingredients" INNER JOIN "ingredients_items" ON "ingredients"."id" = "ingredients_items"."ingredient_id" WHERE "ingredients_items"."item_id" = $1  [["item_id", 139]]
    SQL (3.3ms)  DELETE FROM "ingredients_items" WHERE "ingredients_items"."item_id" = $1 AND "ingredients_items"."ingredient_id" IN (54, 36, 41, 27, 28, 55, 57, 61, 62, 56, 59, 60, 58, 38)  [["item_id", 139]]


## [Factory_girl][3]

    FactoryGirl.define do
      factory :company do
        #company attributes
      end

      factory :user do
       companies {[FactoryGirl.create(:company)]}
       #user attributes
      end

    end


    company = FactoryGirl.create(:company, #{company attributes})
    user = FactoryGirl.create(:user, :companies => [company])

[1]:http://guides.rubyonrails.org/association_basics.html#methods-added-by-has-and-belongs-to-many-collection-delete-object
[2]: http://guides.rubyonrails.org/association_basics.html#scopes-for-has-and-belongs-to-many-distinct
[3]: http://stackoverflow.com/questions/1484374/how-to-create-has-and-belongs-to-many-associations-in-factory-girl
