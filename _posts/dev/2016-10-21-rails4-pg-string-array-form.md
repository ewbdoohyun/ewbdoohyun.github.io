---
title: Rails4에서 PG String Array type 사용하기
layout: post
comments: true
category: [dev, rails]
--- 

### migration

    class AddInstagramCodesToItem < ActiveRecord::Migration
        def change
            add_column :items, :instagram_codes, :string, array: true
        end
    end


### form

    <div class="form-group">
        <%= f.label :instagram_codes, "Instagram", class: "col-sm-2 control-label" %>
        <div class="col-sm-10" id="item-price-selection-list">
          <%= f.text_field :instagram_codes, class: "form-control"%>
        </div>
    </div>


### controller-1

    private
    
    def item_params
        params.require(:item).permit(:instagram_codes, ...)
    end

#controller-2

    def create
        if @item.save
            @item.update_attribute(:instagram_codes,item_params[:instagram_codes].split(", ").map {|c| c.strip})
            ...
    end

    def update
        if @item.update_attributes(item_params)
            @item.update_attribute(:instagram_codes,item_params[:instagram_codes].split(", ").map {|c| c.strip})
            ...
    end


Array로 타입으로 선언한 attribute 값을 쓸 때는 array로 넘겨야 값이 써진다.
string으로 넘겼더니 기존 값도 사라지고 blank array가 되더라.
