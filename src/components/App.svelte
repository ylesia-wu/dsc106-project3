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
            // console.log(d);
            data.push({
                // value: d[columns[i]],
                state: d['State'],
                numDrivers: d['Number of drivers involved in fatal collisions per billion miles'],
                percentSpeeding: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Speeding'],
                percentAlcohol: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Alcohol-Impaired'],
                percentNotDistracted: d['Percentage Of Drivers Invovled In Fatal Collisions Who Were Not Distracted'],
                percentNoPrevious: d['Percentage Of Drivers Invovled In Fatal Collisions Who Had Not Been Involved In Any Previous Accidents']
            })
        }
        });
        data = data;
    });
</script>

<main>
    <!-- Add HTML elements here -->
</main>

<style>
    main {
        
    }
</style>

