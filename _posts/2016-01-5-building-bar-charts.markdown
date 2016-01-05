---
layout: post
title:  "Building Bar Charts with D3.js"
date:   2016-01-05
categories: code
subtext: Making charts with ease.
---
## The Full Pen
<p data-height="668" data-theme-id="0" data-slug-hash="LGxmQa" data-default-tab="result" data-user="qkombur" class='codepen'>See the Pen <a href='http://codepen.io/qkombur/pen/LGxmQa/'>D3.js Bar Graph GDP</a> by Nicholas Auger (<a href='http://codepen.io/qkombur'>@qkombur</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Setting up the document, is very simple.
[D3.js](http://d3js.org/) creates it's graphics by spawning SVG elements. 
Here is a very basic HTML layout for this graph.

#### HTML


{% highlight html %}
<h1>GDP of the USA</h1>
<svg class="chart"></svg>
{% endhighlight %}

Notice the class **chart**

Here we set up all the styling we are going to want for our SVG chart.

#### CSS

{% highlight scss %}
.chart {
  background-color: #ccc;
  border-radius: 10px;
  display: block;
  margin: 0 auto;
}
.chart rect {
  fill: steelblue;
  
}
.chart rect:hover {
  fill: grey;
}
.chart text {
  fill: black;
  font: 10px sans-serif;
  text-anchor: end;
}
.axis text {
  font: 10px sans-serif;
}

.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}
div.tooltip {   
    position: absolute;         
    text-align: center;         
    width: 60px;                    
    height: 40px;                   
    padding: 2px;               
    font: 12px sans-serif;      
    background: lightsteelblue; 
    border: 0px;        
    border-radius: 8px;         
    pointer-events: none;           
}
h1 {
  text-align: center;
}
{% endhighlight %}


After styling we can start with the JavaScript.


#### JavaScript
{% highlight javascript %}

var url = "https://raw.githubusercontent.com/FreeCodeCamp/ProjectReferenceData/master/GDP-data.json";
//setting up margins and dimensions of the chart.
var margin = {top: 20, right: 0, bottom: 30, left: 90},
    width = 960 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;
//define the tooltip.
var div = d3.select("body").append("div") 
    .attr("class", "tooltip")       
    .style("opacity", 0);
//this sets up the spaces between each bar. Make changes to .1 to see difference.
var x = d3.scale.ordinal()
    .rangeRoundBands([0, width], .1);

var y = d3.scale.linear()
    .range([height, 0]);

//sets up the y axis.
var yAxis = d3.svg.axis()
    .scale(y)
    .orient("left");
// here we spawn the chart.
var chart = d3.select(".chart")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


// here we pull data from the json url and prepare the chart
d3.json(url, function(data) {
//makes future calls easier.
var data = data.data;
  

  //d[0] = data.data[0] this is a date and gives order to the chart.
  x.domain(data.map(function(d) { return d[0]; }));
  //d[1] is the quanity of GDP and this function gives the height of the bar accordingly.
  y.domain([0, d3.max(data, function(d) { return d[1]; })]);
  //here we set the barWidth by dividing the charts width by the data length. This squeezes in the data points.
  var barWidth = width / data.length;

  //this creates the axis on the y.
  chart.append("g")
      .attr("class", "y axis")
      .call(yAxis);
      
// here we select each bar.
  chart.selectAll(".bar") // selecting each bar.
      .data(data) // counting and parsing our data.
    .enter().append("rect") //creating each rect class
      .attr("class", "bar") // setting class to bar
      .attr("x", function(d) { return x(d[0]); }) //for each bar picks a date 
      .attr("y", function(d) { return y(d[1]); }) //and picks a quanity if you remove this it flips the graph upside down fun fact.
      .attr("height", function(d) { return height - y(d[1]); }) //spawns each bar.
      .attr("width", x.rangeBand()) //spwans the width.
      .on("mouseover", function(d) { // Here we set the mouse over function.
            div.transition()    //setting up animation.
                .duration(200)    
                .style("opacity", .9);    
            div.html(d[0] + "<br/>"  +  "&#36;" + d[1]) //selects the correct info on each bar.
                .style("left", (d3.event.pageX) + "px") 
                .style("top", (d3.event.pageY - 28) + "px");  
            })          
        .on("mouseout", function(d) {   //removes the tooltip when your mouse leaves the bar.
            div.transition()    
                .duration(500)    
                .style("opacity", 0); 
        });
});
  

{% endhighlight %}


Very brief post, just wanted to share this quick project.