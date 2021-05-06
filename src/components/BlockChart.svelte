<script>
    export let data;
    import {groups, sort, ascending} from "d3-array";

    let dataByYear = groups(data, d => d.year).sort(ascending)
</script>

<section id="blockchart">
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
</section>

<style>
    #blockchart {
        max-width: 50em;
        margin: 0 auto;
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