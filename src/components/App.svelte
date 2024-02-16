<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    // import Temperature from './Temperature.svelte';
    // import Temperature01Marks from './Temperature01Marks.svelte';
    // import Temperature02Channels from './Temperature02Channels.svelte';
    // import Temperature03Axes from './Temperature03Axes.svelte';
    // import Temperature04Interactive from './Temperature04Interactive.svelte';

    // <!-- load csv data -->
    let data = [];

    onMount(async () => {
        const res = await fetch(
        'bad-drivers.csv',
        );
        const csv = await res.text();
        console.log(csv);

        // await d3.csvParse(csv, (d, i, columns) => {
        // // for (let i = 1; i < 13; ++i) {
        //     // console.log(d[i]);

        // //     // pivot longer
        // //     // data.push({
            
        // //     // date: new Date(Date.UTC(d.Year, i - 1, 1)),
        // //     // value: +d[columns[i]],
        // //     // });
        // // }
        // });
        // data = data;
    });











</script>

<main>
    <!-- Add HTML elements here -->
</main>

<style>
    main {
        
    }
</style>

