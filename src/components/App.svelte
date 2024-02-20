<script>

    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    // <!-- load csv data -->

    let state = [];
    let numDrivers = [];
    let speeding = [];
    let alcohol = [];
    let notDistracted = [];
    let noPrevious = [];

    onMount(async () => {
        const res = await fetch('bad-drivers.csv');
        const csv = await res.text();

        await d3.csvParse(csv, (d, i, columns) => {

                state.push({state: d['State'], value: 0});
                numDrivers.push({state: d['State'], value: d['Number of drivers involved in fatal collisions per billion miles']});
                speeding.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Were Speeding']})
                alcohol.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired']});
                notDistracted.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Were Not Distracted']});
                noPrevious.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents']})

        });
        state = state;
        numDrivers = numDrivers;
        speeding = speeding;
        alcohol = alcohol;
        notDistracted = notDistracted;
        noPrevious = noPrevious;
    });

    let currentData = state;

    let xScale;
    let yScale;

    // set the dimensions and margins of the graph
    const margin = {top: 60, right: 40, bottom: 120, left: 60};
    const width = 1300;
    const height = 680;

    // Set up scales on component mount
   
    $: xScale = d3.scaleBand()
                .domain(currentData.map(d => d.state))
                .range([margin.left, width - margin.right]) // Adjust the range as needed
                .padding(0.1);

    let y_label = '';
    let subtitle = '';

    function update(selectedData) {
        currentData = selectedData;

        if (selectedData === numDrivers) {
            y_label = 'Number of Drivers Involved in Fatal Collisions Per Billion Miles';
            subtitle = 'Number of Drivers Involved in Fatal Collisions Per Billion Miles in 50 US States';
        } else if (selectedData === speeding) {
            y_label = 'Drivers Involved In Fatal Collisions Who Were Speeding (%)';
            subtitle = 'Drivers Involved In Fatal Collisions Who Were Speeding (%) in 50 US States';
        } else if (selectedData === alcohol) {
            y_label = 'Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired (%)';
            subtitle = 'Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired (%) in 50 US States';

        } else if (selectedData === notDistracted) {
            y_label = 'Drivers Involved In Fatal Collisions Who Were Not Distracte (%)';
            subtitle = 'Drivers Involved In Fatal Collisions Who Were Not Distracte (%) in 50 US States';
        } else if (selectedData === noPrevious) {
            y_label = 'Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents (%)';
            subtitle = 'Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents (%) in 50 US States';

        } else {
            y_label = 'Default Y-Axis Label';
        }
        
    }

    let xAxis;
    let yAxis;

    $: {
        yScale = d3.scaleLinear()
            .domain([0, Math.max(25, d3.max(currentData, d => d.value))])
            .range([height - margin.bottom, margin.top]); // Adjust the range as needed
        
        d3.select(yAxis).call(d3.axisLeft(yScale));
        
        d3.select(xAxis)
            .call(d3.axisBottom(xScale))
            .selectAll('.tick > text')
            .attr('y', 0)
            .attr('dy', '0.35em')
            .attr('dx', '-1em')
            .attr('text-anchor', 'end')
            .attr('transform', 'rotate(-90)');
    }

</script>

<main>

    <!-- Add 5 buttons -->
    <button on:click={() => update(numDrivers)}>Total Number</button>
    <button on:click={() => update(speeding)}>Speeding</button>
    <button on:click={() => update(alcohol)}>Alcohol</button>
    <button on:click={() => update(notDistracted)}>Not Distracted</button>
    <button on:click={() => update(noPrevious)}>No Previous Accident</button>

    <svg {width} {height}>
    
        
        <g class="x-axis">
            <text transform={`translate(${width / 2}, ${height - 10})`} text-anchor="middle">US States</text>
            <!-- You can add tick marks for x-axis if needed -->
        </g>

        <g class="y-axis">
            <text transform={`translate(${margin.left / 2}, ${height / 2}) rotate(-90)`} text-anchor="middle">{y_label}</text>
            

        <!-- bars -->
        <g class="bars">
			{#each currentData as data}
				<rect	
                    x={xScale(data.state)}
                    y={yScale(data.value)}
                    width={xScale.bandwidth()}
                    height={height - margin.bottom - yScale(data.value)}
				/>
			{/each}
		</g>

        <g transform="translate(0, {height - margin.bottom})"
        bind:this={xAxis} />
        
        <g transform="translate({margin.left}, 0)"
        bind:this={yAxis} />

        <!-- Title -->
        <text x="50%" y="30" font-size="24" text-anchor="middle">Good Drivers? Bad Drivers?</text>
    
        <!-- Subtitle -->
        <text x="50%" y="60" font-size="16" text-anchor="middle">{subtitle}</text>

    </svg>



</main>

<style>
    
</style>

