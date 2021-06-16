<script>
    import {groups, sort, ascending} from "d3";
import { validate_each_argument } from "svelte/internal";
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
                {#each copy.prose4 as value, i}
                    <div class="step" data-step="{i}" id="step-{i}">
                        <div class="step-container">
                            <p class="song-song">{@html value.song}</p>
                            {#if value.lyrics !== undefined}
                                <p class="song-lyrics">{@html value.lyrics}</p>
                            {/if}
                        </div>
                    </div>
                {/each}
        </div>
    </div>
    <div class="tooltip"></div>
</section>

<style>
    #blockchart {
        max-width: 60em;
        margin: 0 auto;
    }

    #scroll {
        position: relative;
        width: 100%;
    }

    .scroll__graphic {
        width: 100%;
        position: sticky;
        top: 3rem;
        right: 0;
        margin: 0 2rem 0 0;
        padding: 0 2rem;
    }

    .scroll__text {
        position: relative;
        top: 0;
        max-width: 35rem;
        width: 100%;
        padding: 0 2rem;
        margin: 1rem auto;
        z-index: 1000;
    }

    .step.is-active {
        opacity: 1;
    }

    #step-8 {
        pointer-events: none;
        opacity: 0;
    }

    .step {
        margin: 2rem auto;
		opacity: 0.5; 
    }

    .step-container {
        background-color: rgba(255, 255, 255, 0.95);
        padding: 2rem;
        box-shadow: 0.25rem 0.25rem 2rem rgba(var(--off-black), 0.125);
    }

    .song-year {
        font-size: 1.25rem;	
        font-weight: 700;
        margin: 0;
    }

    .song-song {
        font-size: 1.5rem;	
        line-height: 1.75;
        margin: 0;
    }

    .song-lyrics {
        font-style: italic;
        font-size: 1.25rem;
        margin: 2rem 0 0 0;
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
        width: calc(100% - 3rem);
        justify-content: space-between;
    }

    .song {
        width: 4px;
        margin: 0 2px;
        height: 2rem;
        background: rgba(255,255,255,0.5);
        pointer-events: none;
        transition: width 0.5s ease-in;
        background-size: 2rem 2rem;
    }

    .song-highlight {
        background-color: yellow;
        outline: 2px solid var(--off-black);
    }

    .is-highlighted {
        width: 2rem;
        background-size: 2rem 2rem;
    }

    @media only screen and (max-width: 900px) {
        .song {
            width: 3px;
            margin: 0 1px;
        }

        .is-highlighted {
            width: 2rem;
            background-size: 2rem 2rem;
        }
    }

    @media only screen and (max-width: 700px) {
        .song {
            width: 1px;
            margin: 0 1px;
        }

        .is-highlighted {
            width: 2rem;
            background-size: 2rem 2rem;
        }
    }

    @media only screen and (max-width: 400px) {
        .ranking {
            margin: 0 0 0 2.25rem;
            font-size: 0.75rem;
        }
        .scroll__graphic {
            margin: 0;
            padding: 0 1rem;
        }
        .year p {
            font-size: 0.75rem;
            width: 2.25rem;
        }
        .song {
            width: 1px;
            margin: 0 1px;
        }
        .is-highlighted {
            width: 2rem;
        }
        .song-song {
            font-size: 1.25rem;
        }

        .song-lyrics {
            font-size: 1rem;
        }
    }
</style>