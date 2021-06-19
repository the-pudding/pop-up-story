<script>
    import {select, selectAll} from "d3";

    export let data;
    let songs;

    function songClick(artist, song) {
        artist[2] = song.song;
        artist[3] = song.sampleLyrics;
        data = data;

        const artistNoSpaces = artist[0].replace(/[^A-Z0-9]/ig, '')
        songs = selectAll(`#card-${artistNoSpaces} .song`)
        songs.classed("selected", false);

        const clickedItem = event.target;
        clickedItem.classList.add("selected")
    }
</script>

<section id="smallchart">
    <div class="key">
        <p class="same-gender">Same- & opposite-gender lyrics</p>
        <p class="opposite-gender">Opposite-gender lyrics only</p>
        <p class="nb-relationship">Non-binary artist & masculine pronoun lyrics</p>
        <p class="unspecified-gender">Unspecified gender lyrics</p>
        <p class="no-relationship">No relationship lyrics</p>
    </div>
    <div class="cards">
        {#each data as artist, i}
            {#if artist[1].length > 4}
            <div class="card" id="card-{artist[0].replace(/[^A-Z0-9]/ig, '')}">
                <div class="img-wrapper">
                    <img src="./assets/images/{artist[0].replace(/[^A-Z0-9]/ig, '').toLowerCase()}.png" alt="{artist[0]} portrait">
                </div>
                <p class="name">{artist[0]}</p>
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
                        <p class="lyrics">🎵{@html artist[3]}🎵</p>
                    </div>
                {:else}
                    <div class="tooltip">
                        <p class="title">"{artist[1][0].song}"</p>
                        <p class="lyrics">🎵{@html artist[1][0].sampleLyrics}🎵</p>
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
        z-index: 999;
    }

    .block {
        width: 1.125rem;
        height: 1.125rem;
        margin: 0 0.25rem 0 0;
    }

    .key p {
        margin: 0;
        padding: 0.5rem 0.5rem;
        width: 20%;
        font-weight: 700;
        display: flex;
        text-align: center;
        align-items: center;
        justify-content: center;
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

    .img-wrapper:hover {
        transform: rotate(10deg);
    }

    .tooltip p {
        font-weight: 700;
        text-align: center;
        margin: 1rem 0;
        font-size: 1.25rem;
    }

    .name {
        font-family: var(--anton);
        text-transform: uppercase;
        font-size: 1.5rem;
        text-align: center;
        height: 4.5rem;
        display: flex;
        align-items: center;
        justify-content: center;
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

    @media only screen and (max-width: 1050px) {
        .key {
            height: 6rem;
        }
        .key p {
            width: 50%;
        }
    }

    @media only screen and (max-width: 800px) {
        .key p {
            font-size: 0.8rem;
        }
    }

    @media only screen and (max-width: 600px) {
        .key {
            height: 10rem;
        }
        .key p {
            width: 100%;
        }
    }
</style>