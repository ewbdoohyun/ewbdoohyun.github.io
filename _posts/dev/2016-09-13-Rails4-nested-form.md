---
title: Rails4 - nested form
layout: post
comments: true
category: [dev, rails]
--- 

- `육아는 아이템전`을 만들면서 사용한 nested form에 대해 정리해 둔다.
- Parenting_neees_money에서 각 Item은 여러개의 keyword를 가지고 있고, keyword는 여러개의 data 로그를 가지고 있다.

    {% highlight ruby %}
    class Item < ActiveRecord::Base
        has_many :keywords, dependent: :destroy
    end

    class Keyword < ActiveRecord::Base
        belongs_to :item
        has_many :logs, dependent: :destroy
    end

    class Logs < ActiveRecord::Base
        belongs_to :keyword
    end
    {% endhighlight %}

- Item을 추가하는 form에서 keyword도 추가하고 싶고, keyword에 대한 log도 3rd party api를 통해 가져와서 생성해야 했다.
- Rails에서는 form에 association을 위한 field를 만들 수 있다.
- 우선 `Item` model에 `accepts_nested_attributes_for :keywords, allow_destroy: true` 를 추가한다.
    {% highlight ruby %}
    class Item < ActiveRecord::Base
        has_many :keywords, dependent: :destroy
        accepts_nested_attributes_for :keywords, allow_destroy: true
    end
    {% endhighlight %}
- `accepts_nested_attributes_for :keywords` 는 `keywords_attributes=` method를 추가시켜준다.
- 기본적으로 create/update가 가능하고 `allow_destroy: true` option을 통해서 destroy도 가능하게 할 수 있다.
- 이제 strong parameter에서 `keywords_attributes`를 허용시켜주면 item.create 또는 item.udpate_attributes method를 통해 Keyword 를 create/update 할 수 있게 된다.
    {% highlight ruby %}
    class ItemsController < ApplicationController
    private

      def item_params
        params.require(:item).permit(keywords_attributes: [:id, :name, '_destroy')
      end
    end    
    {% endhighlight %}
- `allow_destroy: true` 옵션을 사용하려면 strong parameter에 `id, _destroy` 를 허용시켜 줘야 한다.
- `_destroy`의 값은 `marked_for_destruction?` 값에 대입되고, 해당 값이 `true`이면, item object가 `save` 될 때, Keyword record가 삭제된다.
- `accepts_nested_attributes_for` 는 원하는 만큼 depth를 만들 수 있다.
- Item form을 통해서 Keywrod, Log record를 만들어야 되기에 Keyword model에도 accepts_nested_attributes_for를 추가해준다.
    {% highlight ruby %}
    class Keyword < ActiveRecord::Base
        has_many :logs, dependent: :destroy
        accepts_nested_attributes_for :logs
    end
    {% endhighlight %}
- 이제 Keyword model에 `logs_attributes=` method가 추가되었다.
- Item controller의 strong parameter에 logs_attributes를 추가로 허용해 준다.
    {% highlight ruby %}
    class ItemsController < ApplicationController
      private

      def item_params
        params.require(:item).permit(keywords_attributes: [:id, :name, '_destroy', logs_attributes: [:count, :log_date]]))
      end
    end        
    {% endhighlight %}
- `app/views/items/edit.html.erb` 에 keyword field를 추가한다.
    {% highlight ruby %}
    <%= form_for @item do |f| %>
    ...
    <%= f.fields_for :keywords, :wrapper => false do |builder| %>
      <%= render 'keyword_fields', f: builder %>
    <% end %>
    {% endhighlight %}
- `:wrapper => false `는 기본적으로 생성되는 `<div class="fields">` 를 없애준다.
    - table 내부에 추가할 때는 이 옵션을 사용한다.[참고][nested_form]
- `app/views/items/_keyword_fields.html.erb`
    {% highlight ruby %}
    <%= f.text_field :name %>
    <%= link_to_add_log f, object_id %>
    <%= f.hidden_field :_destroy %>
    <%= link_to "remove", "#", class: "remove_keyword" %>
    {% endhighlight %}
- remove link를 클릭하면 javascript로 hidden_field인 `_destroy`의 값을 `1`로 세팅하고 해당 form을 보이지 않게한다. `app/assets/javascripts/items.js`
    {% highlight javascript %}
    $(document).on("turbolinks:load", function(){
        $("form").on("click", ".remove_keyword", function(event){
            $(this).prev('input[type=hidden]').val('1');
            $(this).closest('tr').hide();
            event.preventDefault();
        });
    });
    {% endhighlight %}
- `app/helpers/items_helper.rb`에 `link_to_add_log` 메소드를 추가했다. Log는 해당 keyword에 대해 3rd party API를 통해 얻은 data를 기록한다.
    {% highlight ruby %}
    module ItemsHelper
      def link_to_add_log(f)
        new_object = Log.new
        id = new_object.object_id
        fields = f.fields_for(:logs, new_object, child_index: id) do |builder|
            render("log_field", f: builder)
        end
        link_to("Add Log", '#', class: "update_nsa_data", data: {id: id, fields: fields.gsub("\n", "")})
      end
    end
    {% endhighlight %}
- 미리 _Log_ form을 `Add Log` 링크의 data attribute에 추가시켜둔다. javscript로 해당 링크를 클릭했을 때, data attribute에 있는 form을 dom에 붙여준다.
- `child_index: id`로 keyword의 id가 아닌 _Log_ instance의 id를 사용하게 한다. id는 임의로 object_id를 사용하고, 실제 `Add Log` 링크를 눌러 _Log_ form이 추가될 때, javascript 를 이용, 시간에 의해 생성된 unique id로 바꿔준다.
- `app/views/items/_log_field.html.erb`
    {% highlight ruby %}
    <%= f.text_field :nmc %>
    <%= f.text_field :npc %>
    <%= f.text_field :log_date %>
    {% endhighlight %}
- `app/assests/javascripts/items.js`
    {% highlight javascript %}
    $('form').on('click', '.update_nsa_data', function(e){
        var update_btn = $(this)
        event.preventDefault();
        // add new nsa_dat form
        var time = new Date().getTime();
        var regexp = new RegExp($(this).data('id'), 'g');
        $(this).after($(this).data('fields').replace(regexp, time));

        // get 3rd party data and fill in input fields
    });
    {% endhighlight %}


---

[Rails github - nested_attributes.rb](https://github.com/rails/rails/blob/master/activerecord/lib/active_record/nested_attributes.rb)
[RailsCast 196-nested-model-form](http://railscasts.com/episodes/196-nested-model-form-revised?view=asciicast)

[nested_form]: https://github.com/ryanb/nested_form/wiki/How-To:-Render-nested-fields-inside-a-table
