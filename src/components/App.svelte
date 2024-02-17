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

                // state.push({
                //     state: d['State'],
                //     numDrivers: d['Number of drivers involved in fatal collisions per billion miles'],
                //     speeding: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Speeding'],
                //     alcohol: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Alcohol-Impaired'],
                //     notDistracted: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Not Distracted'],
                //     noPrevious: d['Percentage Of Drivers Invovled In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents']
                // })
                state.push(d['State']);
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

    let currentData = numDrivers;

    let xScale;
    let yScale;

    // set the dimensions and margins of the graph
    const margin = {top: 60, right: 20, bottom: 120, left: 60};
    const width = 1300;
    const height = 600;



    // Set up scales on component mount
    // onMount(() => {
        // Initialize scales
    $: xScale = d3.scaleBand()
                .domain(currentData.map(d => d.state))
                .range([margin.left, width - margin.right]) // Adjust the range as needed
                .padding(0.1);

    $: yScale = d3.scaleLinear()
                .domain([0, d3.max(currentData, (d) => d.value)])
                .range([height - margin.bottom, margin.top]); // Adjust the range as needed
    // });

    function update(selectedData) {
        currentData = selectedData;

    }

    let xAxis;
    let yAxis;

    $: {
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
    <!-- <button on:click={() => function (d) {currentData = numDrivers}}>Total Number</button> -->
    <button on:click={() => update(speeding)}>Speeding</button>
    <button on:click={() => update(alcohol)}>Alcohol</button>
    <button on:click={() => update(notDistracted)}>Not Distracted</button>
    <button on:click={() => update(noPrevious)}>No Previous Accident</button>

    <svg {width} {height}>
    

        <!-- x axis -->
		<!-- <g class="axis x-axis">
			{#each currentData as data}
				<text
                    x={xScale(data.state)}
                    y={height}
                    text-anchor="middle"
                    rotate="-90"
                    font-size="10"
                >
                    {data.state}
                </text>
			{/each}
		</g>  -->

        <!-- y axis -->
		<!-- <g class="axis y-axis">
			<line x1="0" y1="400" x2="0" y2="0" stroke="black" />
            {#each yScale.ticks() as tick}
                <g transform="translate(0, {yScale(tick)})">
                    <line x1="-5" y1="0" x2="0" y2="0" stroke="black" />
                    <text x="-10" y="0" text-anchor="end" alignment-baseline="middle">{tick}</text>
                </g>
            {/each}
		</g> -->
        <!-- {console.log('Inside for loop:', numDrivers)} -->
        
        <!-- bars -->
        <!-- <g class="bars"> -->
			{#each currentData as data}
				<rect	
                    x={xScale(data.state)}
                    y={height - yScale(data.value)}
                    width={xScale.bandwidth()}
                    height={yScale(data.value)}
				/>
			{/each}
		<!-- </g> -->

        <g transform="translate(0, {height - margin.bottom})"
        bind:this={xAxis} />
        
        <g transform="translate({margin.left}, 0)"
        bind:this={yAxis} />

    </svg>

    
    <!-- <button
        aria-label="Total Number"
        on:click={() => update(numDrivers)}
        class="update"
    />
    <button
        aria-label="Speeding"
        on:click={() => update(speeding)}
        class="update"
    />
    <button
        aria-label="Alcohol"
        on:click={() => update(alcohol)}
        class="update"
    />
    <button
        aria-label="NotDistracted"
        on:click={() => update(notDistracted)}
        class="update"
    />
    <button
            aria-label="NoPrevious"
            on:click={() => update(noPrevious)}
            class="update"
    /> -->

</main>

<!-- <style>
    rect {
       fill: blue;  
    }
</style> -->

