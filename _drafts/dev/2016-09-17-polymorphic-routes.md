

{% highlight ruby %}
<% elements.each do |element| %>
    <li class="item-nav-list">
    	<%= link_to polymorphic_path(element) do %>
    		<%= element.name  %>
    		<span class="table-mask"></span>
    	<% end %>
    </li>
  <% end %>
{% endhighlight %}



http://api.rubyonrails.org/classes/ActionDispatch/Routing/PolymorphicRoutes.html