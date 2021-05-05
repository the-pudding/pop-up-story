<script>
    export let data;

    import {groups, sort, ascending} from "d3-array";


    let dataByYear = groups(data, d => d.year).sort(ascending)

    console.log(dataByYear);
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
        height: 40px;
        display: flex;
        flex-direction: row;
        margin: 0 0 1rem 0;
    }

    .year p {
        width: 3rem;
    }

    .year-wrapper {
        display: flex;
        flex-direction: row;
        width: calc(100% - 3rem);
    }

    .song {
        width: 6px;
        margin: 0 1px;
        height: 50px;
        background-color: var(--gray-light);
    }

    .song-highlight {
        background-color: red;
    }

</style>