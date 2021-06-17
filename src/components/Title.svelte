<script>
    export let copy;

    import { onMount } from 'svelte';
    import inView from "../actions/inView.js";
    import {selectAll} from "d3";

    let visibleChapstick = false;
    let chapstickDivs;

    onMount(() => {
        chapstickDivs = selectAll("#chapstick div");
	});

    function showChapstick() {
        visibleChapstick = !visibleChapstick
        if (visibleChapstick) { chapstickDivs.classed("animation", true) }
        else { chapstickDivs.classed("animation", false) }
    }
</script>

<section id="title"
    use:inView
    on:enter={() => showChapstick()}
    on:exit={() => showChapstick()}
>
    <h1>{copy[0].headline}</h1>
    <h2>{copy[0].subhed}</h2>
    <div>
        <p class="byline byline-bold">{@html copy[0].byline1}</p>
        <p class="byline">{@html copy[0].byline2}</p>
    </div>
</section>

<style>
   #title {
        max-width: 55em;
        margin: 0 auto 5rem auto;
        padding: 1rem;
   }

   h1 {
       text-transform: uppercase;
       font-weight: 700;
       font-family: var(--anton);
       font-size: 7.5rem;
       margin: 0 0 1rem 0;
       padding: 0;
       line-height: 1;
       text-align: center;
   }

   h2 {
       font-size: 2rem;
       text-align: center;
       max-width: 45rem;
       margin: 0 auto;
   }

   .byline {
       text-align: center;
   }

   .byline-bold {
       font-weight: 700;
   }

   @media only screen and (max-width: 700px) {
        h1 {
            font-size: 7.5rem;
        }

        h2 {
            font-size: 1.75rem;
        }
    }

   @media only screen and (max-width: 450px) {
        h1 {
            font-size: 5.25rem;
        }

        h2 {
            font-size: 1.5rem;
        }
    }
</style>