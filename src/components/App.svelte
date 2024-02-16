<script>

    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    // <!-- load csv data -->
    let data = [];

    onMount(async () => {
        const res = await fetch('bad-drivers.csv');
        const csv = await res.text();

        await d3.csvParse(csv, (d, i, columns) => {
            for (let i = 1; i < 2; ++i) {
                data.push({
                    state: d['State'],
                    numDrivers: d['Number of drivers involved in fatal collisions per billion miles'],
                    speeding: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Speeding'],
                    alcohol: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Alcohol-Impaired'],
                    notDistracted: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Not Distracted'],
                    noPrevious: d['Percentage Of Drivers Invovled In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents']
                })
            }
        });
        data = data;
    });

    let xScale;
    let yScale;

    // set the dimensions and margins of the graph
    // const margin = {top: 30, right: 30, bottom: 30, left: 30};
    // const width = 400;
    // const height = 400;

    // let currentData = data.map(d => ({state: d.state, value: d.numDrivers}));

    // Set up scales on component mount
    // onMount(() => {
    //     // Initialize scales
    //     xScale = scaleBand()
    //     .domain(currentData.map(d => d.state))
    //     .range([0, 400]) // Adjust the range as needed
    //     .padding(0.1);

    //     yScale = scaleLinear()
    //     .domain([0, d3.max(currentData, d => d.value)])
    //     .range([400, 0]); // Adjust the range as needed
    // });

    function update(selectedVar) {
        
        currentVar = selectedVar;

        // currentData = data.map(d => ({state: d.state, value: d.selectedVar}));

        // yScale = scaleLinear()
        //         .domain([0, d3.max(currentData, d => d.value)])
        //         .range([400, 0]);

    }

</script>

<main>

    <!-- Add 5 buttons -->
    <button on:click={() => update('numDrivers')}>Total Number</button>
    <button on:click={() => update('speeding')}>Speeding</button>
    <button on:click={() => update('alcohol')}>Alcohol</button>
    <button on:click={() => update('notDistrcted')}>Not Distracted</button>
    <button on:click={() => update('noPrevious')}>No Previous Accident</button>

    <svg width=400 height=400>
        <!-- x axis -->
		<g class="axis x-axis">
			{#each currentData as d}
				<text
                    x={xScale(d.state) + xScale.bandwidth() / 2}
                    y="15"
                    text-anchor="middle"
                    font-size="12"
                >
                    {d.state}
                </text>
			{/each}
		</g>

        y axis
		<g class="axis y-axis">
			<line x1="0" y1="400" x2="0" y2="0" stroke="black" />
            {#each yScale.ticks() as tick}
                <g transform="translate(0, {yScale(tick)})">
                    <line x1="-5" y1="0" x2="0" y2="0" stroke="black" />
                    <text x="-10" y="0" text-anchor="end" alignment-baseline="middle">{tick}</text>
                </g>
            {/each}
		</g>

        <!-- bars
        <g class="bars">
			{#each currentData as state, value}
				<rect
					x={xScale(state)}
					y={yScale(value)}
					width={8}
					height={yScale(0) - yScale(value)}
				/>
			{/each}
		</g> -->
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

<style>
    main {
         
    }
</style>

