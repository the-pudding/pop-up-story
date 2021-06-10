<script>
    import { onMount } from 'svelte';
    import {selectAll} from "d3";

    export let data;
    export let type;
    let songs;

    // MOUNT
    onMount(() => {
    
    });

    function songClick(artist, song) {
        artist[2] = song.song;
        artist[3] = song.sampleLyrics;
        data = data;

        const artistNoSpaces = artist[0].replace(/\s/g, '')
        songs = selectAll(`#card-${artistNoSpaces} .song`)
        songs.classed("selected", false);

        const clickedItem = event.target;
        clickedItem.classList.add("selected")
    }
</script>

<section id="smallchart">
    <div class="key">
        {#if type == "cis"}
            <p class="same-gender">Same- & opposite-gender lyrics</p>
            <p class="opposite-gender">Opposite-gender lyrics only</p>
            <p class="unspecified-gender">Unspecified gender lyricss</p>
            <p class="no-relationship">No relationship lyrics</p>
        {:else}
            <p class="nb-relationship">Masculine pronoun lyrics</p>
            <p class="unspecified-gender">Unspecified gender lyrics</p>
            <p class="no-relationship">No relationship lyrics</p>
        {/if}
    </div>
    <div class="cards">
        {#each data as artist, i}
            {#if artist[1].length > 4}
            <div class="card" id="card-{artist[0].replace(/\s/g, '')}">
                <div class="img-wrapper">
                    <img src="./assets/images/{artist[0].replace(/[^A-Z0-9]/ig, '').toLowerCase()}.png" alt="{artist[0]} portrait">
                </div>
                <p>{artist[0]}</p>
                <div class="songs">
                    {#each artist[1] as song, i}
                        {#if i == 0}
                            <div class="song selected song-{song.relationshipType}" data-artist={song.artist} data-song={song.song} data-lyrics={song.sampleLyrics}
                                on:click={() => songClick(artist, song, i) }>
                            </div>
                        {:else}
                        <div class="song song-{song.relationshipType}" data-artist={song.artist} data-song={song.song} data-lyrics={song.sampleLyrics}
                            on:click={() => songClick(artist, song, i) }>
                        </div>
                        {/if}
                    {/each}
                </div>
                {#if artist.length > 2}
                    <div class="tooltip">
                        <p class="title">"{artist[2]}"</p>
                        <p class="lyrics">🎵{artist[3]}🎵</p>
                    </div>
                {:else}
                    <div class="tooltip">
                        <p class="title">"{artist[1][0].song}"</p>
                        <p class="lyrics">🎵{artist[1][0].sampleLyrics}🎵</p>
                    </div>
                {/if}
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

    .song-Queer, .song-Both {
        outline: none;
        background-color: yellow;
    }

    .same-gender {
        background-color: yellow;
    }

    .song-Non-Binary-Masc {
        background-color: lime;
    }

    .song-Opposite, .opposite-gender {
        background-color: cyan;
    }

    .song-Unspecified, .unspecified-gender {
        background-color: magenta;
    }

    .no-relationship {
        background: var(--gray-light);
    }

    .nb-relationship {
        background: lime;
    }

    .song:hover, .song.selected {
        outline: 2px solid var(--off-black);
    }
    
    .key {
        width: 100%;
        display: flex;
        flex-direction: row;
        margin: 0 auto;
        justify-content: center;
        position: sticky;
        height: 2.5rem;
        top: 0;
        flex-wrap: wrap;
    }

    .block {
        width: 1.125rem;
        height: 1.125rem;
        margin: 0 0.25rem 0 0;
    }

    .key p {
        margin: 0;
        padding: 0.5rem 0;
        width: 25%;
        text-align: center;
        font-weight: 700;
    }

    .cards {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        width: 100%;
        padding: 0 1rem;
        max-width: 60rem;
        margin: 2rem auto;
        justify-content: center;
    }

    .card {
        max-width: 15rem;
        background-color: rgba(255, 255, 255, 0.5);
        margin: 1rem;
        padding: 2rem;
        display: flex;
        flex-direction: column;
    }
    .img-wrapper {
        width: 8rem;
        height: 8rem;
        margin: 0 auto;
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
        cursor: pointer;
    }

    .tooltip .title {
        font-weight: 700;
        font-size: 1rem;
    }

    .tooltip .lyrics {
        font-weight: 500;
        font-style: italic;
        font-size: 1rem;
    }

    @media only screen and (max-width: 400px) {
        .key p {
            width: 50%;
            font-size: 0.65rem;
        }
    }
</style>