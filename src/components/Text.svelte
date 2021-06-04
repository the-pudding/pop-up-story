<script>
    export let copy;
    export let section;

    let sectionNum = parseInt(section);
    let visibleChapstick = false;
    let chapstickDivs;

    let visibleBestBuy = false;
    let buildingDiv;
    let dvdDiv;
    let flagDiv;
    
    import { onMount } from 'svelte';
    import inView from "../actions/inView.js";
    import {select, selectAll} from "d3";

    onMount(() => {
        chapstickDivs = selectAll("#chapstick div");
        buildingDiv = selectAll("#bestbuy .left-scene .building");
        dvdDiv = selectAll("#bestbuy .dvd");
        flagDiv = selectAll("#bestbuy .flag");
	});

    function showChapstick() {
        visibleChapstick = !visibleChapstick
        if (visibleChapstick) { chapstickDivs.classed("animation", true) }
        else { chapstickDivs.classed("animation", false) }
    }

    function animationSequence() {
        if (visibleBestBuy) { 
            buildingDiv.classed("on-screen", true) 
            buildingDiv.classed("off-screen", false) 
            dvdDiv.classed("on-screen", true) 
            dvdDiv.classed("off-screen", false) 
        }
        else { 
            buildingDiv.classed("on-screen", false) 
            buildingDiv.classed("off-screen", true)
            dvdDiv.classed("on-screen", false) 
            dvdDiv.classed("off-screen", true)  
        }
        visibleBestBuy = !visibleBestBuy
    }
</script>

{#if sectionNum == 1}
<section class="text"
    use:inView
    on:enter={() => showChapstick()}
    on:exit={() => showChapstick()}
>
    <div class="prose">
        {#each copy as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
</section>
{:else if sectionNum == 2}
<section class="text"
    use:inView
    on:enter={() => animationSequence()}
    on:exit={() => animationSequence()}
>
    <div class="prose">
        {#each copy as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
</section>
{:else if sectionNum == 7}
<section class="method">
    <h3>Methods & Notes</h3>
    <div class="prose">
        {#each copy as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
</section>
{:else}
<section class="text">
    <div class="prose">
        {#each copy as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
</section>
{/if}

<style>
    .prose {
        max-width: 40em;
        margin: 0 auto;
    }

    p {
        font-size: 1.5rem;
        margin: 2rem 0;
    }

    .method {
        padding: 5rem 0 0 0;
    }

    h3 {
        max-width: 40em;
        margin: 0 auto;
        font-size: 1.25rem;
        font-weight: 700; 
    }

    .method p {
        font-size: 1rem;
    }
</style>