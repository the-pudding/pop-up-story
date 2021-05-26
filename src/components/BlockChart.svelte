<script>
    import { onMount } from 'svelte';
    import {selection, select, selectAll} from "d3-selection";
    import {groups, sort, ascending} from "d3-array";
    import copy from "../data/doc.json";

    export let data;
    let dataByYear = groups(data, d => d.year).sort(ascending);
</script>

<section id="blockchart">
    <div id="scroll">
        <div class="scroll__graphic sticky">
            <div>
                <div class="ranking">
                    <p>#1</p>
                    <p>#100</p>
                </div>
                {#each dataByYear as year, i}
                    <div class="year year-{year[0]}">
                        <p>{year[0]}</p>
                        <div class="year-wrapper">
                            {#each year[1] as song}
                                {#if song.lyricFlag == 1}
                                    <div class="song song-highlight {song.artist}"></div>
                                {:else}
                                    <div class="song"></div>
                                {/if}
                            {/each}
                        </div>
                    </div>
                {/each}
            </div>
        </div>
        <div class='scroll__text'>
                {#each copy.prose4 as {value}, i}
                    <div class="step" data-step="{i}" id="step-{i}">
                        <p class>{@html value}</p>
                    </div>
                {/each}
        </div>
    </div>
    <div class="tooltip"></div>
</section>

<style>
    #blockchart {
        max-width: 50em;
        margin: 0 auto;
    }

    #scroll {
        position: relative
    }

    .scroll__graphic {
        width: 100%;
        position: sticky;
        top: 3rem;
        right: 0;
        transform: translate3d(0,0,0)
    }

    .scroll__text {
        position: relative;
        top: 0;
        max-width: 35rem;
        width: 100%;
        padding: 0 2rem;
        margin: 1rem auto;
        pointer-events: none;
        z-index: 1000;
    }

    .step.is-active {
        opacity: 1;
    }

    .step {
        margin: 2rem auto;
		opacity: 0.5; 
    }

    .step p {
        background-color: rgba(255, 255, 255, 0.75);
        font-size: 1.5rem;	
        padding: 1rem;
        box-shadow: 0.25rem 0.25rem 2rem rgba(var(--off-black), 0.125);	
        line-height: 1.75;
    }

    .tooltip {
        position: absolute;
    }

    .ranking {
        width: calc(100% - 3rem);
        margin: 0 0 0 3rem;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    .year {
        width: 100%;
        height: 30px;
        display: flex;
        flex-direction: row;
        margin: 0 0 0.5rem 0;
    }

    .year p {
        width: 3rem;
        margin: 0;
    }

    .year-wrapper {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        width: calc(100% - 3rem);
    }

    .song {
        width: 5px;
        height: 30px;
        background-color: rgba(255, 255, 255, 0.8);
    }

    .song-highlight {
        background-color: red;
    }

    .song:hover {
        outline: 2px solid var(--off-black);
    }

</style>