<script>
    import { onMount } from 'svelte';
    import {selection, select, selectAll} from "d3-selection";
    import {groups, sort, ascending, filter, rollup} from "d3-array";
    import copy from "../data/doc.json";

    export let data;
    let dataQueerArtists = data.filter(d => d.queerFlag == 1);
    dataQueerArtists = dataQueerArtists.sort((a,b) => ascending(a.relationshipType, b.relationshipType))
    let dataQueerSpotlight = groups(dataQueerArtists, d => d.artistID)
    let dataSpotlightCount = rollup(dataQueerArtists, v => v.length, d => d.artistID)
    console.log(dataQueerSpotlight)
</script>

<section id="smallchart">
    <div class="wrapper">
        {#each dataQueerArtists as song, i}
            {#if song.relationshipType == "Both" || song.relationshipType == "Queer"}
                <div class="song song-highlight"></div>
            {:else}
                <div class="song"></div>
            {/if}
        {/each}
    </div>
    <div class="cards">
        {#each dataQueerSpotlight as artist, i}
            {#if artist[1].length > 4}
            <div class="card">
                <p>{artist[0]}</p>
                <div class="songs">
                    {#each artist[1] as song, i}
                        {#if song.relationshipType == "Both" || song.relationshipType == "Queer"}
                            <div class="song song-queer"></div>
                        {:else if song.relationshipType == "Straight"}
                            <div class="song song-straight"></div>
                        {:else if song.relationshipType == "Unspecified"}
                            <div class="song song-unspecified"></div>
                        {:else}
                            <div class="song song-none"></div>
                        {/if}
                    {/each}
                </div>
            </div>
            {/if}
        {/each}
    </div>
    <div class="tooltip"></div>
</section>

<style>
    #smallchart {
        margin: 0 auto;
    }

    .wrapper {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        width: 100%;
        padding: 0 1rem;
    }

    .song {
        width: 4px;
        height: 30px;
        background-color: rgba(255, 255, 255, 0.8);
    }

    .song-highlight {
        background-color: red;
    }

    .song-queer {
        background-color: red;
    }

    .song-straight {
        background-color: blue;
    }

    .song-unspecified {
        background-color: green;
    }

    .song:hover {
        outline: 2px solid var(--off-black);
    }

    .cards {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
    }

    .card {
        width: 20rem;
    }

    .songs {
        display: flex;
        flex-direction: row;
    }

</style>