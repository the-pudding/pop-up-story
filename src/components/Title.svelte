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
        max-width: 40em;
        margin: 0 auto 5rem auto;
   }

   h1 {
       text-transform: uppercase;
       font-weight: 700;
       font-family: var(--anton);
       font-size: 12rem;
       margin: 0;
       padding: 0;
       line-height: 1;
   }

   .byline-bold {
       font-weight: 700;
   }
</style>