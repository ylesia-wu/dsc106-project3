<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    // <!-- load csv data -->
    let data = [];

    onMount(async () => {
        const res = await fetch(
                                'bad-drivers.csv',
                                );
        const csv = await res.text();

        await d3.csvParse(csv, (d, i, columns) => {
        for (let i = 1; i < 2; ++i) {
            data.push({
                state: d['State'],
                numDrivers: d['Number of drivers involved in fatal collisions per billion miles'],
                Speeding: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Speeding'],
                Alcohol: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Alcohol-Impaired'],
                NotDistracted: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Not Distracted'],
                NoPrevious: d['Percentage Of Drivers Invovled In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents']
            })
        }
        });
        data = data;
    });

    // set the dimensions and margins of the graph
    var margin = {top: 30, right: 30, bottom: 70, left: 60},
        width = 460 - margin.left - margin.right,
        height = 400 - margin.top - margin.bottom;

    // append the svg object to the body of the page
    var svg = d3.select("my_dataviz")
            .append("svg")
                .attr("width", width + margin.left + margin.right)
                .attr("height", height + margin.top + margin.bottom)
            .append("g")
                .attr("transform",
                    "translate(" + margin.left + "," + margin.top + ")");

    // Initialize the X axis
    var x = d3.scaleBand()
              .range([ 0, width ])
              .padding(0.2);
    var xAxis = svg.append("g")
                   .attr("transform", "translate(0," + height + ")")

    // Initialize the Y axis
    var y = d3.scaleLinear()
    .range([ height, 0]);
    var yAxis = svg.append("g")
    .attr("class", "myYaxis")

    function update(selectedVar) {
        // Add Y axis
        y.domain([0, d3.max(data, function(d) { return +d[selectedVar] }) ]);
        yAxis.transition().duration(1000).call(d3.axisLeft(y));
    }

    // Initialize plot
    update('numDrivers')
</script>

<main>
    <!-- Add HTML elements here -->
    <!-- Add 5 buttons -->
    <button onclick="update('numDrivers')">Total Number</button>
    <button onclick="update('Speeding')">Speeding</button>
    <button onclick="update('Alcohol')">Alcohol</button>
    <button onclick="update('NotDistracted')">Not Distracted</button>
    <button onclick="update('NoPrevious')">No Previous Accidents</button>

    <!-- Create a div where the graph will take place -->
    <div id="my_dataviz"></div>
</main>

<style>
    main {
         
    }
</style>

