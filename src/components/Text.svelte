<script>
    export let copy;
    export let section;

    let sectionNum = parseInt(section);
    
    import { onMount } from 'svelte';
    import inView from "../actions/inView.js";
    import {select} from "d3";
    
    let poleDancing = false;
    let lilNasX;
    let bubble;

    onMount(() => {
        lilNasX = select("#pole .lil-nas-x");
        bubble = select("#pole .bubble");
	});

    function animationIn3() {
        lilNasX.classed("with-animation", true);
        lilNasX.style("top", "30vh");
        setTimeout(function() {
            bubble.style("opacity", 1);
        }, 1000);
        poleDancing = !poleDancing;
    }

    function animationOut3() {
        if (poleDancing) {
            lilNasX.classed("with-animation", false);
            lilNasX.style("top", "70vh");
            bubble.style("opacity", 0);
        }
    }

    function animationIn5() {
        lilNasX.classed("with-animation", true);
        lilNasX.style("top", "30vh");
        bubble.select("p").text("testing")
        setTimeout(function() {
            bubble.style("opacity", 1);
        }, 1000);
        poleDancing = !poleDancing;
    }

    function animationOut5() {
        if (poleDancing) {
            lilNasX.classed("with-animation", false);
            lilNasX.style("top", "70vh");
            bubble.style("opacity", 0);
        }
    }

    function animationIn7() {
        lilNasX.style("top", "100vh");
        poleDancing = !poleDancing;
    }

    function animationOut7() {
        if (poleDancing) {
            lilNasX.classed("with-animation", false);
            lilNasX.style("top", "70vh");
        }
    }
</script>


{#if sectionNum == 3}
<section class="text" id="text-{sectionNum}"
    use:inView
    on:enter={() => animationIn3()}
    on:exit={() => animationOut3()}
>
    <div class="prose">
        {#each copy as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
</section>
{:else if sectionNum == 5}
<section class="text" id="text-{sectionNum}"
    use:inView
    on:enter={() => animationIn5()}
    on:exit={() => animationOut5()}
>
    <div class="prose">
        {#each copy as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
</section>
{:else if sectionNum == 7}
<section class="method" id="method-anchor"
    use:inView
    on:enter={() => animationIn7()}
    on:exit={() => animationOut7()}
>
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
        padding: 0 1rem;
    }

    p {
        font-size: 1.5rem;
        line-height: 2.25rem;
        margin: 2rem 0;
    }

    .method {
        padding: 5rem 0 0 0;
    }

    h3 {
        max-width: 32em;
        margin: 0 auto;
        font-size: 1.25rem;
        font-weight: 700; 
        padding: 0 1rem;
    }

    .method p {
        font-size: 1rem;
    }

    @media only screen and (max-width: 450px) {
        p {
            font-size: 1.25rem;
        }
    }
</style>