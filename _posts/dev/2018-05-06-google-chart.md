---
title: Google Chart 로 그래프 그리기
layout: post
comments: true
category: [dev, google-chart]
--- 


## Gem 'google_visualr'

Google chart를 사용할 수 있는 gem이 여러개 있는데, 그중에 google_visualr를 사용한 이유는 'stepped_area_chart' 같은
새로운 chart를 지원하고 있는 gem이어서 선택하게 되었다.

사용법은 간단한데,
1. datatable에 data를 넣고
2. option을 설정하고
3. 원하는 차트 class의 object를 만들 때, data_table과 option을 넘겨주고
4. view에서는 `render_chart`를 

<controller>

    data_table = GoogleVisualr::DataTable.new
    data_table.new_column('string', 'date')
    data_table.new_column('number', 'rank')

    data_table.add_rows(rows)

    option = {...}

    @chart = GoogleVisualr::Interactive::SteppedAreaChart.new(data_table, option)

    respond_to do |format|
      format.js
    end


<view>
	
	$('.item-ranks__chart').append('<%= j render_chart(@chart, "item-ranks__chart") %>');


---
