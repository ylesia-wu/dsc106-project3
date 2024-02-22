<script>

    import { onMount } from 'svelte';
    import { fly } from "svelte/transition"
    import * as d3 from 'd3';

    let state = [];
    let numDrivers = [];
    let speeding = [];
    let alcohol = [];
    let notDistracted = [];
    let noPrevious = [];
    let numDriversSorted = [];
    let speedingSorted = [];
    let alcoholSorted = [];
    let notDistractedSorted = [];
    let noPreviousSorted = [];

    let svg;

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

        const resNumDriversSorted = await fetch('num-driver-sorted.csv');
        const csvNumDriversSorted = await resNumDriversSorted.text();
        await d3.csvParse(csvNumDriversSorted, (d, i, columns) => {

                numDriversSorted.push({state: d['State'], value: d['Number of drivers involved in fatal collisions per billion miles']});
        });
        numDriversSorted = numDriversSorted;
        
        const resSpeedingSorted = await fetch('speeding-sorted.csv');
        const csvSpeedingSorted = await resSpeedingSorted.text();
        await d3.csvParse(csvSpeedingSorted, (d, i, columns) => {
            speedingSorted.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Were Speeding']});
        });
        speedingSorted = speedingSorted;

        const resAlcoholSorted = await fetch('alcohol-sorted.csv');
        const csvAlcoholSorted = await resAlcoholSorted.text();
        await d3.csvParse(csvAlcoholSorted, (d, i, columns) => {
            alcoholSorted.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired']});
        });
        alcoholSorted = alcoholSorted;

        const resNotDistractedSorted = await fetch('not-distracted-sorted.csv');
        const csvNotDistractedSorted = await resNotDistractedSorted.text();
        await d3.csvParse(csvNotDistractedSorted, (d, i, columns) => {
            notDistractedSorted.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Were Not Distracted']});
        });
        notDistractedSorted = notDistractedSorted;

        const resNoPreviousSorted = await fetch('no-previous-sorted.csv');
        const csvNoPreviousSorted = await resNoPreviousSorted.text();
        await d3.csvParse(csvNoPreviousSorted, (d, i, columns) => {
            noPreviousSorted.push({state: d['State'], value: d['Percentage Of Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents']});
        });
        noPreviousSorted = noPreviousSorted;
        
        update(numDrivers, false);
        currentData = numDrivers;
    });

    let currentData = state;

    // set the dimensions and margins of the graph
    const margin = {top: 100, right: 40, bottom: 110, left: 80};
    const width = 1260;
    const height = 650;

    let y_label = '';
    let subtitle = '';
    let avgValue = 15.79;
    let sortClicked = false;

    let circleX1 = 269;
    let circleY1 = 460;
    let lineX1 = 300;
    let lineY1 = 160;
    let textX1 = 230;
    let textY1 = 100;
    let rectX1 = 220;
    let rectY1 = 90;
    let height1 = 50;
    let width1 = 150;
    let annotationText11 = 'D.C. is known for its developed urban';
    let annotationText12 = 'infrastructure, lower speed limits,';
    let annotationText13 = 'and public transportation options.';
    let line13 = true;

    let circleX2 = null;
    let circleY2 = null;
    let lineX2 = null;
    let lineY2 = null;
    let textX2 = null;
    let textY2 = null;
    let rectX2 = null;
    let rectY2 = null;
    let height2 = null;
    let width2 = null;
    let annotationText21 = null;
    let annotationText22 = null;
    let annotationText23 = null;
    let annotationText24 = null;
    let annotationText25 = null;
    let annotationText26 = null;
    let annotation2 = false;
    let line25 = false;
    let line26 = false;

    function update(selectedData, sort) {

        sortClicked = sort;

        if (sort) {
            if ((selectedData === numDrivers) | (selectedData === numDriversSorted)) {
                currentData = numDriversSorted;
                y_label = 'Number of Drivers Involved in Fatal Collisions Per Billion Miles';
                subtitle = 'Number of Drivers Involved in Fatal Collisions Per Billion Miles in 50 US States';
                avgValue = 15.79
                circleX1 = 92;
                circleY1 = 460;
                lineX1 = 250;
                lineY1 = 165;
                textX1 = 200;
                textY1 = 100;
                rectX1 = 220;
                rectY1 = 90;
                height1 = 50;
                width1 = 150;
                annotationText11 = 'D.C. is known for its developed urban';
                annotationText12 = 'infrastructure, lower speed limits,';
                annotationText13 = 'and public transportation options.';
                line13 = true;     
                circleX2 = null;
                circleY2 = null;
                lineX2 = null;
                lineY2 = null;
                textX2 = null;
                textY2 = null;
                rectX2 = null;
                rectY2 = null;
                height2 = null;
                width2 = null;
                annotationText21 = null;
                annotationText22 = null;
                annotationText23 = null;
                annotationText24 = null;
                annotationText25 = null;
                annotationText26 = null;
                annotation2 = false;
                line25 = false;
                line26 = false;
            } else if ((selectedData === speeding) | (selectedData === speedingSorted)) {
                currentData = speedingSorted;
                y_label = 'Drivers Involved In Fatal Collisions Who Were Speeding (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Were Speeding (%) in 50 US States';
                avgValue = 31.73;
                circleX1 = 1220;
                circleY1 = 305;
                lineX1 = 1100;
                lineY1 = 400;
                textX1 = 880;
                textY1 = 400;
                rectX1 = 870;
                rectY1 = 395;
                height1 = 80;
                width1 = 275;
                annotationText11 = 'As one might expect, on average, ';
                annotationText12 = 'around one-third of the drivers involved';
                annotationText13 = 'in fatal collisions were speeding.';
                line13 = true;     
                circleX2 = 1194;
                circleY2 = 130;
                lineX2 = 900;
                lineY2 = 140;
                textX2 = 680;
                textY2 = 110;
                rectX2 = 665;
                rectY2 = 135;
                height2 = 70;
                width2 = 245;
                annotationText21 = 'One possible factor contributing';
                annotationText22 = 'to Hawaii’s higher percentage of';
                annotationText23 = 'speeding drivers in fatal collisions';
                annotationText24 = 'could be local tourism.';
                annotationText25 = null;
                annotationText26 = null;
                annotation2 = true;
                line25 = false;
                line26 = false;
                
            } else if ((selectedData === alcohol) | (selectedData === alcoholSorted)) {
                currentData = alcoholSorted;
                y_label = 'Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired (%) in 50 US States';
                avgValue = 30.69;
                circleX1 = 92;
                circleY1 = 400;
                lineX1 = 672;
                lineY1 = 410;
                textX1 = 680;
                textY1 = 400;
                rectX1 = 670;
                rectY1 = 395;
                height1 = 60;
                width1 = 263;
                annotationText11 = 'Utah considers alcohol-impaired';
                annotationText12 = 'driving to be a highly serious offense.';
                annotationText13 = null
                line13 = false;     
                circleX2 = 1194;
                circleY2 = 130;
                lineX2 = 1040;
                lineY2 = 170;
                textX2 = 780;
                textY2 = 110;
                rectX2 = 775;
                rectY2 = 105;
                height2 = 115;
                width2 = 275;
                annotationText21 = 'One possible factor contributing to';
                annotationText22 = 'Montana’s higher percentage of ';
                annotationText23 = 'alcohol-impaired drivers in fatal';
                annotationText24 = 'collisions could be the drinking culture';
                annotationText25 = 'and higher rates of alcohol consumption.';
                annotationText26 = null;
                annotation2 = true;
                line25 = true;
                line26 = false;
            } else if ((selectedData === notDistracted) | (selectedData === notDistractedSorted)) {
                currentData = notDistractedSorted;
                y_label = 'Drivers Involved In Fatal Collisions Who Were Not Distracted (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Were Not Distracted (%) in 50 US States';
                avgValue = 85.92;
                circleX1 = 1210;
                circleY1 = 180;
                lineX1 = 1130;
                lineY1 = 260;
                textX1 = 880;
                textY1 = 250;
                rectX1 = 870;
                rectY1 = 245;
                height1 = 60;
                width1 = 273;
                annotationText11 = 'Drivers involved in fatal collisions';
                annotationText12 = 'usually are, surprisingly, not distracted.';
                annotationText13 = null
                line13 = false;     
                circleX2 = 93;
                circleY2 = 510;
                lineX2 = 210;
                lineY2 = 385;
                textX2 = 220;
                textY2 = 280;
                rectX2 = 205;
                rectY2 = 275;
                height2 = 120;
                width2 = 285;
                annotationText21 = 'A small percentage of non-distracted';
                annotationText22 = 'drivers suggests that drivers involved in';
                annotationText23 = 'fatal collisions are more often distracted. ';
                annotationText24 = 'One possible factor could be that this';
                annotationText25 = 'state encompasses more rural regions.';
                annotationText26 = null;
                annotation2 = true;
                line25 = true;
                line26 = false;
            } else if ((selectedData === noPrevious) | (selectedData === noPreviousSorted)) {
                currentData = noPreviousSorted;
                y_label = 'Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents (%) in 50 US States';
                avgValue = 88.73;
                circleX1 = 1205;
                circleY1 = 165;
                lineX1 = 1140;
                lineY1 = 200;
                textX1 = 900;
                textY1 = 200;
                rectX1 = 890;
                rectY1 = 190;
                height1 = 65;
                width1 = 260;
                annotationText11 = 'Fatal collisions are usually the first ';
                annotationText12 = 'accident for drivers involved in them.';
                annotationText13 = null;
                line13 = false;     
                circleX2 = null;
                circleY2 = null;
                lineX2 = null;
                lineY2 = null;
                textX2 = null;
                textY2 = null;
                rectX2 = null;
                rectY2 = null;
                height2 = null;
                width2 = null;
                annotationText21 = null;
                annotationText22 = null;
                annotationText23 = null;
                annotationText24 = null;
                annotationText25 = null;
                annotationText26 = null;
                annotation2 = false;
                line25 = false;
                line26 = false;
            } else {
                y_label = ' ';
            }
        } else if (!sort){
            if ((selectedData === numDrivers) | (selectedData === numDriversSorted)) {
                currentData = numDrivers;
                y_label = 'Number of Drivers Involved in Fatal Collisions Per Billion Miles';
                subtitle = 'Number of Drivers Involved in Fatal Collisions Per Billion Miles in 50 US States';
                avgValue = 15.79
                circleX1 = 269;
                circleY1 = 460;
                lineX1 = 250;
                lineY1 = 165;
                textX1 = 200;
                textY1 = 100;
                rectX1 = 220;
                rectY1 = 90;
                height1 = 70;
                width1 = 260;
                annotationText11 = 'D.C. is known for its developed urban';
                annotationText12 = 'infrastructure, lower speed limits,';
                annotationText13 = 'and public transportation options.';
                line13 = true;     
                circleX2 = null;
                circleY2 = null;
                lineX2 = null;
                lineY2 = null;
                textX2 = null;
                textY2 = null;
                rectX2 = null;
                rectY2 = null;
                height2 = null;
                width2 = null;
                annotationText21 = null;
                annotationText22 = null;
                annotationText23 = null;
                annotationText24 = null;
                annotationText25 = null;
                annotationText26 = null;
                annotation2 = false;
                line25 = false;
                line26 = false;         

            } else if ((selectedData === speeding) | (selectedData === speedingSorted)) {
                currentData = speeding;
                y_label = 'Drivers Involved In Fatal Collisions Who Were Speeding (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Were Speeding (%) in 50 US States';
                avgValue = 31.73;
                circleX1 = 1220;
                circleY1 = 305;
                lineX1 = 1100;
                lineY1 = 400;
                textX1 = 880;
                textY1 = 400;
                rectX1 = 870;
                rectY1 = 395;
                height1 = 80;
                width1 = 275;
                annotationText11 = 'As one might expect, on average, ';
                annotationText12 = 'around one-third of the drivers involved';
                annotationText13 = 'in fatal collisions were speeding.';
                line13 = true;     
                circleX2 = 335;
                circleY2 = 130;
                lineX2 = 670;
                lineY2 = 140;
                textX2 = 680;
                textY2 = 110;
                rectX2 = 665;
                rectY2 = 135;
                height2 = 70;
                width2 = 245;
                annotationText21 = 'One possible factor contributing';
                annotationText22 = 'to Hawaii’s higher percentage of';
                annotationText23 = 'speeding drivers in fatal collisions';
                annotationText24 = 'could be local tourism.';
                annotationText25 = null;
                annotationText26 = null;
                annotation2 = true;
                line25 = false;
                line26 = false;
            } else if ((selectedData === alcohol) | (selectedData === alcoholSorted)) {
                currentData = alcohol;
                y_label = 'Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Were Alcohol-Impaired (%) in 50 US States';
                avgValue = 30.69;
                circleX1 = 1061;
                circleY1 = 400;
                lineX1 = 910;
                lineY1 = 410;
                textX1 = 680;
                textY1 = 400;
                rectX1 = 670;
                rectY1 = 395;
                height1 = 60;
                width1 = 263;
                annotationText11 = 'Utah considers alcohol-impaired';
                annotationText12 = 'driving to be a highly serious offense.';
                annotationText13 = null
                line13 = false;     
                circleX2 = 666;
                circleY2 = 130;
                lineX2 = 770;
                lineY2 = 140;
                textX2 = 780;
                textY2 = 110;
                rectX2 = 775;
                rectY2 = 105;
                height2 = 115;
                width2 = 275;
                annotationText21 = 'One possible factor contributing to';
                annotationText22 = 'Montana’s higher percentage of ';
                annotationText23 = 'alcohol-impaired drivers in fatal';
                annotationText24 = 'collisions could be the drinking culture';
                annotationText25 = 'and higher rates of alcohol consumption.';
                annotationText26 = null;
                annotation2 = true;
                line25 = true;
                line26 = false;
            } else if ((selectedData === notDistracted) | (selectedData === notDistractedSorted)) {
                currentData = notDistracted;
                y_label = 'Drivers Involved In Fatal Collisions Who Were Not Distracted (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Were Not Distracted (%) in 50 US States';
                avgValue = 85.92;
                circleX1 = 1210;
                circleY1 = 180;
                lineX1 = 1130;
                lineY1 = 260;
                textX1 = 880;
                textY1 = 250;
                rectX1 = 870;
                rectY1 = 245;
                height1 = 60;
                width1 = 273;
                annotationText11 = 'Drivers involved in fatal collisions';
                annotationText12 = 'usually are, surprisingly, not distracted.';
                annotationText13 = null
                line13 = false;     
                circleX2 = 620;
                circleY2 = 510;
                lineX2 = 480;
                lineY2 = 380;
                textX2 = 220;
                textY2 = 280;
                rectX2 = 205;
                rectY2 = 275;
                height2 = 120;
                width2 = 285;
                annotationText21 = 'A small percentage of non-distracted';
                annotationText22 = 'drivers suggests that drivers involved in';
                annotationText23 = 'fatal collisions are more often distracted. ';
                annotationText24 = 'One possible factor could be that this';
                annotationText25 = 'state encompasses more rural regions.';
                annotationText26 = null;
                annotation2 = true;
                line25 = true;
                line26 = false;
            } else if ((selectedData === noPrevious) | (selectedData === noPreviousSorted)) {
                currentData = noPrevious;
                y_label = 'Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents (%)';
                subtitle = 'Drivers Involved In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents (%) in 50 US States';
                avgValue = 88.73;
                circleX1 = 1205;
                circleY1 = 165;
                lineX1 = 1140;
                lineY1 = 200;
                textX1 = 900;
                textY1 = 200;
                rectX1 = 890;
                rectY1 = 190;
                height1 = 65;
                width1 = 260;
                annotationText11 = 'Fatal collisions are usually the first ';
                annotationText12 = 'accident for drivers involved in them.';
                annotationText13 = null;
                line13 = false;     
                circleX2 = null;
                circleY2 = null;
                lineX2 = null;
                lineY2 = null;
                textX2 = null;
                textY2 = null;
                rectX2 = null;
                rectY2 = null;
                height2 = null;
                width2 = null;
                annotationText21 = null;
                annotationText22 = null;
                annotationText23 = null;
                annotationText24 = null;
                annotationText25 = null;
                annotationText26 = null;
                annotation2 = false;
                line25 = false;
                line26 = false;
            } else {
                y_label = ' ';
            }
        }
    }

    let xScale;
    let yScale;
    let xAxis;
    let yAxis;

    $: {
        xScale = d3.scaleBand()
                .domain(currentData.map(d => d.state))
                .range([margin.left, width - margin.right - 15])
                .padding(0.1);

        yScale = d3.scaleLinear()
            .domain([0, Math.max(25, d3.max(currentData, d => d.value))])
            .range([height - margin.bottom, margin.top]);
        
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

    let highlightedBar = null;

    function handleHover(i) {
        highlightedBar = i;
    }

    function handleLeave() {
        highlightedBar = null;
        tooltipPt = null;
    }

    let tooltipPt = null;
    let tooltipX = null;
    let tooltipY = null;

    function onPointerMove(event, i) {
        console.log('the function ran')
        tooltipX = xScale(currentData[i].state)
        tooltipY = yScale(currentData[i].value) + 50
        tooltipPt = currentData[i]
        console.log(tooltipPt)
    }

    function onPointerLeave() {
        tooltipPt = null;
        tooltipX = null;
        tooltipY = null;
    }

</script>

<main>
    <div style="position: absolute; top: 10px; left: 10px;">
        <!-- Add 5 buttons -->
        <button on:click={() => update(numDrivers, false)}>Total Number</button>
        <button on:click={() => update(speeding, false)}>Speeding</button>
        <button on:click={() => update(alcohol, false)}>Alcohol</button>
        <button on:click={() => update(notDistracted, false)}>Not Distracted</button>
        <button on:click={() => update(noPrevious, false)}>No Previous Accident</button>
    </div>

    <svg id="svgContainer" bind:this={svg} {width} {height} viewBox="0 0 {width} {height}"
    style="width: auto; max-height: 100%;">
    
        <!-- bars -->
        <g class="bars">
			{#each currentData as data, i}
                {#if sortClicked}
                    <rect	
                        x={xScale(data.state)}
                        y={yScale(data.value)}
                        width={xScale.bandwidth()}
                        height={height - margin.bottom - yScale(data.value)}
                        in:fly = {{x: -200, duration: 5, delay: i * 10}}
                        class:highlighted={highlightedBar === i}
                        on:mouseover={(event) => 
                                        {handleHover(i);
                                        onPointerMove(event, i);}}
                        on:mouseout={(event) => 
                                        {handleLeave();
                                        onPointerLeave();}}
                             
                    />
                {:else}
                    <rect  
                        x={xScale(data.state)}
                        y={yScale(data.value)}
                        width={xScale.bandwidth()}
                        height={height - margin.bottom - yScale(data.value)}
                        class:highlighted={highlightedBar === i}
                        on:mouseover={(event) => 
                            {handleHover(i);
                            onPointerMove(event, i);}}
                        on:mouseout={(event) => 
                                        {handleLeave();
                                        onPointerLeave();}}
                        
                    />
                {/if}
            {/each} 
		</g>
        
        <g class="axis-title">
            <text transform={`translate(${width / 2}, ${height})`} text-anchor="middle">US States</text>
            <!-- You can add tick marks for x-axis if needed -->
        </g>

        <g class="axis-title">
            <text transform={`translate(${margin.left / 2}, ${height / 2}) rotate(-90)`} text-anchor="middle">{y_label}</text>
            
            <!-- Draw a line for the average -->
            <line x1="{margin.left + 1}" y1="{yScale(avgValue)}" x2="{width - margin.right - 17}" y2="{yScale(avgValue)}" stroke="#f27a11" stroke-width="2" />
            <!-- Add a label for the average -->
            <text x="{width - margin.right - 7}" y="{yScale(avgValue)}" font-size="12" fill="#f27a11">Average:</text>
            <text x="{width - margin.right - 7}" y="{yScale(avgValue) + 15}" font-size="12" fill="#f27a11">{avgValue}</text>
        </g> 
        
        <g transform="translate(0, {height - margin.bottom})"
        bind:this={xAxis} />
        
        <g transform="translate({margin.left}, 0)"
        bind:this={yAxis} />

        <!-- Title -->
        <text x="50%" y="58" font-size="24" text-anchor="middle" class="title">Good Drivers? Bad Drivers?</text>
    
        <!-- Subtitle -->
        <text x="50%" y="78" font-size="16" text-anchor="middle" class="subtitle">{subtitle}</text>
        
        <!-- tooltip -->
        {#if tooltipPt}
            <g transform="translate({tooltipX},{tooltipY})">
                <rect x="-70" y="-20" width="140" height="55" fill="white" stroke="black"></rect>
                <text x="0" y="0" font-size="14" font-weight="regular" font-family="Arial, sans-serif" text-anchor="middle" dominant-baseline="central">{tooltipPt.state}</text>
                <text x="0" y="20" font-size="14" font-weight="regular" font-family="Arial, sans-serif" text-anchor="middle" dominant-baseline="central">{tooltipPt.value}</text>
            </g>
        {/if}
        
        <g>
            <rect x={rectX1} y={rectY1} width={width1} height={height1} fill="white" opacity="0.7" />
            <!-- Circle -->
            <circle cx={circleX1} cy={circleY1} r="4" fill="black" />
            <line x1={circleX1} y1={circleY1} x2={lineX1} y2={lineY1} stroke="black" />
            <!-- Text -->
            <text x={textX1} y={textY1} font-size="font-size" font-family="Arial, sans-serif">
                <tspan x={textX1} dy="1.2em">{annotationText11}</tspan>
                <tspan x={textX1} dy="1.2em">{annotationText12}</tspan>
                {#if line13}
                <tspan x={textX1} dy="1.2em">{annotationText13}</tspan>
                {/if}
            </text>
        </g>

        {#if annotation2}
        <g>
            <rect x={rectX2} y={rectY2} width={width2} height={height2} fill="white" opacity="0.7" />
            <!-- Circle -->
            <circle cx={circleX2} cy={circleY2} r="4" fill="black" />
            <line x1={circleX2} y1={circleY2} x2={lineX2} y2={lineY2} stroke="black" />
            <!-- Text -->
            <text x={textX2} y={textY2} font-size="font-size" font-family="Arial, sans-serif">
                <tspan x={textX2} dy="1.2em">{annotationText21}</tspan>
                <tspan x={textX2} dy="1.2em">{annotationText22}</tspan>
                <tspan x={textX2} dy="1.2em">{annotationText23}</tspan>
                <tspan x={textX2} dy="1.2em">{annotationText24}</tspan>
                {#if line25}
                <tspan x={textX2} dy="1.2em">{annotationText25}</tspan>
                {/if}
                {#if line26}
                <tspan x={textX2} dy="1.2em">{annotationText26}</tspan>
                {/if}
            </text>
        </g>
        {/if}
    </svg>

    <button style="position: absolute; bottom: 30px; left: 40px;" on:click={() => update(currentData, true)}>Sort</button>
    <button style="position: absolute; bottom: 30px; left: 84px;" on:click={() => update(currentData, false)}>Undo Sort</button>

</main>

<style>
    .title {
        font-family: Arial, sans-serif;
    }

    .subtitle {
        font-family: Arial, sans-serif;
    }

    .axis-title {
        font-family: Arial, sans-serif;
        font-size: 14px;
    }

    .bars {
        fill: #a7a8a8;
    }

    .highlighted {
        fill: #f27a11;
    }
</style>

