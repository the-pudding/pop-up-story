<script>
    export let copy;
    import BlockChart from "./BlockChart.svelte";
    import data from "../data/sheet.csv";
</script>

<section id="text">
    <div class="prose">
        {#each copy.prose1 as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
    <BlockChart data="{data}"/>
    <div class="prose">
        {#each copy.prose2 as {value}}
            <p class>{@html value}</p>
        {/each}
    </div>
</section>

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