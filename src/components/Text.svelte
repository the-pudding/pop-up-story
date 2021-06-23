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
    let pole;
    let w;

    onMount(() => {
        lilNasX = select("#pole .lil-nas-x");
        bubble = select("#pole .bubble");
        pole = select("#pole .line");

        console.log(w)
	});

    function animationIn3() {
        pole.classed("full-height", true);
        lilNasX.classed("with-animation", true);
        lilNasX.style("top", "30vh");
        poleDancing = !poleDancing;
    }

    function animationOut3() {
        if (poleDancing) {
            lilNasX.classed("with-animation", false);
            lilNasX.style("top", "80vh");
            if (w < 700) { bubble.style("top", "-100px"); }
            setTimeout(function() {
                bubble.style("opacity", 1);
            }, 1000);
        }
    }

    function animationIn5() {
        pole.classed("full-height", true);
        lilNasX.classed("with-animation", true);
        lilNasX.style("top", "30vh");
        bubble.style("opacity", 0);
        poleDancing = !poleDancing;
    }

    function animationOut5() {
        if (poleDancing) {
            lilNasX.classed("with-animation", false);
            lilNasX.style("top", "80vh");
            if (w < 700) { bubble.style("top", "-130px"); }
            else { bubble.style("top", "-110px"); }
            bubble.select(".text").text("y’all love saying i’m being gay for success but can’t name 5 successful gay male artists in the last 10 years to save your life")
            bubble.select("a").attr("href", "https://twitter.com/LilNasX/status/1384516504310272000?s=20")
            setTimeout(function() {
                bubble.style("opacity", 1);
            }, 1000);
        }
    }

    function animationIn7() {
        pole.classed("full-height", true);
        bubble.style("opacity", 0);
        setTimeout(function() {
            bubble.style("opacity", 1);
            if (w < 700) { bubble.style("top", "-80px"); }
            else { bubble.style("top", "-60px"); }
        bubble.select(".text").text("happy pride month. i will be having sex with 100 lucky fans to celebrate.")
        bubble.select("a").attr("href", "https://twitter.com/LilNasX/status/1399688353021263872")
        }, 1000);
        poleDancing = !poleDancing;
    }

    function animationOut7() {
        if (poleDancing) {
            lilNasX.style("top", "100vh");
            bubble.style("opacity", 0);
        }
    }
</script>

<svelte:window bind:innerWidth={w}/>

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

    @media only screen and (max-width: 800px) {
        .prose {
            padding: 0 6rem 0 1rem;
        }
    }

    @media only screen and (max-width: 450px) {
        p {
            font-size: 1.25rem;
        }
    }
</style>