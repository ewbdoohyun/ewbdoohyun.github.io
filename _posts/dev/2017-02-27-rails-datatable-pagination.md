---
title: Rails - jQuery DataTable pagination
layout: post
comments: true
category: [dev, rails]
--- 

params로 넘어오는 값이 [railscast github source][2]와 좀 바뀐게 있다.


params[:sSearch] --> params[:search][:value]
params[:iDisplayStart] --> params[:start]
params[:iDisplayLength] --> params[:length]
params[:iSortCol_0] --> params[:order]['0']['column']
params[:sSortDir_0] --> params[:order]['0']['dir']


---

[railscast 340-datatable][1]
[railscast github source][2]

[1]: http://railscasts.com/episodes/340-datatables?autoplay=true
[2]: https://github.com/railscasts/340-datatables/blob/master/store-after/app/datatables/products_datatable.rb
