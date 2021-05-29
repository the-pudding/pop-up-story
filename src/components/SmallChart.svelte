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

    function songClick() {
        console.log(this);
    }
</script>

<section id="smallchart">
    <div class="key">
        <div class="key-block">
            <div class="block same-gender"></div>
            <p>Same-gender</p>
        </div>
        <div class="key-block">
            <div class="block opposite-gender"></div>
            <p>Opposite-gender</p>
        </div>
        <div class="key-block">
            <div class="block unspecified-gender"></div>
            <p>Unspecified gender</p>
        </div>
        <div class="key-block">
            <div class="block no-relationship"></div>
            <p>No relationship</p>
        </div>
    </div>
    <div class="cards">
        {#each dataQueerSpotlight as artist, i}
            {#if artist[1].length > 4}
            <div class="card">
                <div class="img-wrapper"></div>
                <p>{artist[0]}</p>
                <div class="songs">
                    {#each artist[1] as song, i}
                        {#if song.relationshipType == "Both" || song.relationshipType == "Queer"}
                            <div class="song song-queer" on:click={songClick}></div>
                        {:else if song.relationshipType == "Straight"}
                            <div class="song song-straight" on:click={songClick}></div>
                        {:else if song.relationshipType == "Unspecified"}
                            <div class="song song-unspecified" on:click={songClick}></div>
                        {:else}
                            <div class="song song-none" on:click={songClick}></div>
                        {/if}
                    {/each}
                </div>
                <div class="tooltip">
                    <p class="title">"{artist[1][0].song}"</p>
                    <p class="lyrics">{artist[1][0].sampleLyrics}</p>
                </div>
            </div>
            {/if}
        {/each}
    </div>
</section>

<style>
    #smallchart {
        margin: 0 auto;
    }

    .song {
        width: 4px;
        height: 30px;
        background: var(--gray-light);
    }

    .song-queer {
        outline: none;
        background-color: cyan;
    }

    .same-gender {
        background-color: cyan;
    }

    .song-straight, .opposite-gender {
        background-color: yellow;
    }

    .song-unspecified, .unspecified-gender {
        background-color: magenta;
    }

    .no-relationship {
        background: var(--gray-light);
    }

    .song:hover {
        outline: 2px solid var(--off-black);
    }
    
    .key {
        max-width: 60rem;
        display: flex;
        flex-direction: row;
        margin: 0 auto;
        justify-content: center;
    }

    .key-block {
        margin: 0 1rem;
        display: flex;
        flex-direction: row;
        align-items: center;
    }

    .block {
        width: 1.125rem;
        height: 1.125rem;
        margin: 0 0.25rem 0 0;
    }

    .key p {
        margin: 0;
    }

    .cards {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        width: 100%;
        padding: 0 1rem;
        max-width: 60rem;
        margin: 0 auto;
        justify-content: center;
    }

    .card {
        width: 15rem;
        background-color: rgba(255, 255, 255, 0.5);
        margin: 1rem;
        padding: 2rem;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }
    .img-wrapper {
        width: 5rem;
        height: 5rem;
        background-color: var(--gray-light);
        margin: 0 auto;
        border-radius: 50%;
    }

    .card p {
        font-weight: 700;
        text-align: center;
        margin: 1rem 0;
        font-size: 1.25rem;
    }

    .songs {
        display: flex;
        flex-direction: row;
        justify-content: center;
    }

    .card .song {
        margin: 0 0.125rem;
    }

    .tooltip .title {
        font-weight: 500;
        font-size: 1rem;
    }

    .tooltip .lyrics {
        font-weight: 500;
        font-style: italic;
        font-size: 1rem;
    }

</style>