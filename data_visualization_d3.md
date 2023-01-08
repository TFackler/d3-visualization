### Add Document Elements with D3

```html
<body>

</body>
```

```javascript
// most functions in d3 return the object itself for method chaining

d3.select("body") // select the first ul element in the document
    .append("h1") // add a list item inside
    .text("Learning D3"); // set the text of the new element 
```

### Select a Group of Elements with D3

```html
<body>
  <ul>
    <li>Example</li>
    <li>Example</li>
    <li>Example</li>
  </ul>
</body>
```

```javascript
d3.select('ul')
    .selectAll('li')
    .text('Changed text')
```

### Work with Data in D3

```javascript
const dataset = [12, 31, 22, 17, 25, 18, 29, 14, 9];

d3.select('body')
    .selectAll('h2')
    .data(dataset) // binds the data to the 'selected' html nodes
    .enter()    // compares selected nodes with the number of data points given and creates items until
                // they are the same number by running the code after
    .append('h2')
    .text('New Title')
```

```html

<body>

</body>

-> 

<body>
    <h2>New Title</h2>
    <h2>New Title</h2>
    <h2>New Title</h2>
    <h2>New Title</h2>
    <h2>New Title</h2>
    <h2>New Title</h2>
    <h2>New Title</h2>
    <h2>New Title</h2>
    <h2>New Title</h2>
</body>
```

### Work with Dynamic Data in D3

```javascript
const dataset = [12, 31, 22, 17, 25, 18, 29, 14, 9];

d3.select("body").selectAll("h2")
  .data(dataset)
  .enter()
  .append("h2")
  .text((s) => s + ' USD') // supplying text() with a callback function let's you access the bound 
                            // data point
```

### Add Inline Styling to Elements

```javascript
// The style() method takes a comma-separated key-value pair as an argument.
selection.style("color","blue") // Set the selection's text color to blue
```

### Change Styles Based on Data

```javascript
// similar to text() style() accepts a callback to style data-dependent
selection.style('color', (data) => {
    return data < 20 ? 'red' : 'green'
})
```

### Add Classes with D3

```javascript
// puts attributes onto the selection; also supports callbacks
selection.attr("class", "container");
```

### Update the Height of an Element Dynamically

```javascript
const dataset = [12, 31, 22, 17, 25, 18, 29, 14, 9];

d3.select("body").selectAll("div")
  .data(dataset)
  .enter()
  .append("div")
  .attr("class", "bar")
  .style('height', (data) => data * 10) // height dependent on dataset
```

### Learn About SVG in D3

```javascript
// create empty svg to draw in
const svg = d3.select("body")
    .append('svg')
    .attr('width', w)
    .attr('height', h)
```

### Create a SVG bar chart from a dataset

```css
.bar:hover {
    fill: brown;
}
```

```javascript
const dataset = [12, 31, 22, 17, 25, 18, 29, 14, 9];

const w = 500;
const h = 100;

const svg = d3.select("body")
              .append("svg")
              .attr("width", w)
              .attr("height", h);

svg.selectAll("rect") // link rects with dataset
   .data(dataset)
   .enter()
    // create a rect for every bar, set style using svg-rect attributes
    // (d, i) takes in data d and index i
   .append("rect")
   .attr("x", (d, i) => i * 30)
   .attr("y", (d, i) => h - 3 * d) // (0, 0) is the top left corner
   .attr("width", 25)
   .attr("height", (d, i) => d * 3)
   .attr("fill", "navy")
   .attr("class", "bar") // add on-hover style
    // add tooltip
   .append('title')
   .text((d) => d)

// adding labels to the bars
svg.selectAll("text")
   .data(dataset)
   .enter()
   .append("text")
   .text((d) => d)
   .attr("x", (d, i) => i * 30)
   .attr("y", (d, i) => h - (d * 3 + 3))
   .attr('fill', 'red') // svg text color
   .style('font-size', 25) // style
```

### Create a Scatterplot with SVG Circles

```javascript
const dataset = [
                  [ 34,    78 ],
                  [ 109,   280 ],
                  [ 310,   120 ],
                  [ 79,    411 ],
                  [ 420,   220 ],
                  [ 233,   145 ],
                  [ 333,   96 ],
                  [ 222,   333 ],
                  [ 78,    320 ],
                  [ 21,    123 ]
                ];


const w = 500;
const h = 500;

const svg = d3.select("body")
              .append("svg")
              .attr("width", w)
              .attr("height", h);

svg.selectAll("circle")
   .data(dataset)
   .enter()
   .append("circle")
    // cx, cy are the coordinates of the center of the circle, r the radius
   .attr("cx", (d, i) => d[0])
   .attr("cy", (d, i) => h - d[1])
   .attr("r", 5);

svg.selectAll("text")
   .data(dataset)
   .enter()
   .append("text")
   .text((d) => d[0] + ', ' + d[1])
   .attr("x", (d, i) => d[0] + 5)
   .attr("y", (d, i) => h - d[1])
```

### Using a linear scale

```javascript
const scale = d3.scaleLinear();
scale.domain([50, 480]); // the range of the input data
scale.range([10, 500]); // the range of the output (svg size)
scale(50) // 10
scale(480) // 500
scale(325) // 323.37
scale(750) // 807.67
d3.scaleLinear()
```

### Using and showing dynamic scales

```javascript
const dataset = [
              [ 34,    78 ],
              [ 109,   280 ],
              [ 310,   120 ],
              [ 79,    411 ],
              [ 420,   220 ],
              [ 233,   145 ],
              [ 333,   96 ],
              [ 222,   333 ],
              [ 78,    320 ],
              [ 21,    123 ]
            ];

const w = 500;
const h = 500;

// Padding between the SVG canvas boundary and the plot
const padding = 30;

// Create an x and y scale
const xScale = d3.scaleLinear()
                .domain([0, d3.max(dataset, (d) => d[0])])
                .range([padding, w - padding]);

const yScale = d3.scaleLinear()
                .domain([0, d3.max(dataset, (d) => d[1])])
                .range([h - padding, padding]); // interval flipped because of y-drawing coords growing downwards

const output = yScale(411); // Returns 30

// displaying data by positions elements using scaled data
svg.selectAll("circle")
    .data(dataset)
    .enter()
    .append("circle")
    .attr('cx', (d) => xScale(d[0]))
    .attr('cy', (d) => yScale(d[1]))
    .attr('r', 5)

svg.selectAll("text")
    .data(dataset)
    .enter()
    .append("text")
    .text((d) =>  (d[0] + ", " + d[1]))
    .attr('x', (d) => xScale(d[0] + 10))
    .attr('y', (d) => yScale(d[1]))

// draw axes
const xAxis = d3.axisBottom(xScale); // create axis generator function
const yAxis = d3.axisLeft(yScale);


svg.append("g") // add new graphics to svg
    .attr("transform", "translate(0," + (h - padding) + ")") // move graphics to be in view and in the right place
    .call(xAxis); // call axis generator with current selection

svg.append("g")
    .attr("transform", "translate(" + padding + ", 0)")
    .call(yAxis)
```