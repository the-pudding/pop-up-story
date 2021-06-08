<script>
        import { fly } from 'svelte/transition';
        import inView from "../actions/inView.js";
        import "intersection-observer";

        let dvdWidth;
        let bestbuyWidth;
        let carWidth;
        let visible = false;

        function toggleVisible() {
            visible = !visible
        }   
</script>

<section id="bestbuy"
    use:inView
    on:enter={() => toggleVisible()}
    on:exit={() => toggleVisible()}>
    {#if !visible}
        <div class="left-scene">
            <div class="flag"></div>
            <div transition:fly="{{ x: -dvdWidth, duration: 1000, delay: 1000 }}" class="dvd" bind:clientWidth={dvdWidth}></div>
            <div transition:fly="{{ x: -bestbuyWidth, duration: 1000 }}" class="building" bind:clientWidth={bestbuyWidth}></div>
        </div>
        <div class="right-scene">
            <div transition:fly="{{ x: carWidth*5, duration: 3000, delay: 0 }}" class="car" bind:clientWidth={carWidth}></div>
        </div>
    {/if}
</section>

<style>
    #bestbuy {
        width: 100%;
        height: 300px;
        display: flex;
        flex-direction: row;
        position: relative;
    }

    .building {
        position: absolute;
        background-image: url("../assets/images/bestbuy.png");
        width: 400px;
        height: 400px;
        background-size: contain;
        background-repeat: no-repeat;
    }

    .dvd {
        position: absolute;
        left: 0;
        top: 0;
        background-image: url("../assets/images/dvd.jpg");
        width: 100px;
        height: 100px;
        background-size: contain;
        background-repeat: no-repeat;
    }

    .car {
        position: absolute;
        left: 400px;
        bottom: 0;
        background-image: url("../assets/images/sunfire.png");
        width: 250px;
        height: 150px;
        background-size: contain;
        background-repeat: no-repeat;
    }
</style>