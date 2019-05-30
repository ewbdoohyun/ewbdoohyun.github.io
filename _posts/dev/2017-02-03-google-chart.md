https://developers.google.com/chart/interactive/docs/gallery/columnchart





    <script type="text/javascript">
        google.charts.load("current", {packages:['corechart']});
        google.charts.setOnLoadCallback(drawBasic);
        function drawBasic() {
    
          var data = new google.visualization.DataTable();
          data.addColumn('string','Date');
          data.addColumn('number','rank');
          data.addRows(<%= raw @ranks %>);
    
          var view = new google.visualization.DataView(data);
          view.setColumns([0, 1,
                           { calc: function (dt, row) {
                                return (<%= raw @max_rank %> - parseInt(dt.getValue(row, 1)) + 1).toString() + "위";
                                },
                             sourceColumn: 1,
                             type: "string",
                             role: "annotation" }
                           ]);
    
          var options = {
            title: '',
            hAxis: {
              title: '',
              gridlines: {count: 0},
            },
            vAxis: {
              title: '',
              baselineColor: '#eee',
              gridlineColor: '#eee',
              textPosition: 'none'
            },
            width: 150,
            height: 100,
            chartArea: {
              width: 150,
              height: 100,
            },
            legend: { position: "none" },
            annotations: {
              alwaysOutside: true,
              textStyle: {
                fontSize: 10,
                color: '#4f00ff',
              },
              stem: {
                color: '#fff',
              },
              color: '#4f00ff',
            },
            series: { 0: {color: '#4f00ff'} }
          };
    
          var chart = new google.visualization.ColumnChart(
            document.getElementById('rank'));
    
          chart.draw(view, options);
        }
    </script>
    <div id="rank"></div>
