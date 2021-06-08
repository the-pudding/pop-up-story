<script>
    export let copy;
    export let section;

    let sectionNum = parseInt(section);
    
    import { onMount } from 'svelte';
    import inView from "../actions/inView.js";
    import {select, selectAll} from "d3";
    
    let poleDancing = false;
    let lilNasX;
    let padPos = -200;

    onMount(() => {
        lilNasX = select("#pole .lil-nas-x")
	});

    function animationSequence() {
        console.log(poleDancing, lilNasX)

        if (poleDancing) {
            lilNasX.classed("with-animation", true)
        }
        else {
            lilNasX.classed("with-animation", false) 
        }
        
        poleDancing = !poleDancing;
    }
</script>


{#if sectionNum == 3}
<section class="text" id="text-{sectionNum}"
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
{:else if sectionNum == 8}
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
        padding: 0 1rem;
    }

    p {
        font-size: 1.5rem;
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

    @media only screen and (max-width: 400px) {
        p {
            font-size: 1.25rem;
        }
    }
</style>