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
    import {selection, select, selectAll} from "d3-selection";

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
<section id="text"
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
<section id="text"
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
{:else}
<section id="text">
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
</style>