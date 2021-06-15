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
            <div transition:fly="{{ y: 200, duration: 1000, delay: 2000 }}" class="flag"></div>
            <div transition:fly="{{ x: -dvdWidth*5, duration: 1000, delay: 1000 }}" class="dvd" bind:clientWidth={dvdWidth}></div>
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
        background-image: url("../assets/images/bestbuy_drawing.png");
        top: 0;
        width: 40%;
        height: 115%;
        background-size: contain;
        background-repeat: no-repeat;
    }

    .dvd {
        position: absolute;
        left: 30%;
        bottom: 10%;
        background-image: url("../assets/images/dvd_drawing.png");
        width: 10%;
        height: 40%;
        background-size: contain;
        background-repeat: no-repeat;
    }

    .flag {
        position: absolute;
        left: 5%;
        top: -50%;
        background-image: url("../assets/images/flag_drawing.png");
        width: 20%;
        height: 90%;
        background-size: contain;
        background-repeat: no-repeat;
    }

    .car {
        position: absolute;
        left: 28%;
        bottom: 0;
        background-image: url("../assets/images/sunfire_drawing.png");
        width: 35%;
        height: 45%;
        background-size: contain;
        background-repeat: no-repeat;
    }

    @media only screen and (max-width: 700px) {
        #bestbuy {
            height: 400px;
        }

        .building {
            width: 65%;
            top: 10%;
        }

        .dvd {
            left: 65%;
            bottom: 8%;
            width: 8%;
        }

        .flag {
            top: -5%;
            left: 5%;
            width: 15%;
        }

        .car {
            left: 50%;
            width: 40%;
        }
    }

    @media only screen and (max-width: 400px) { 
        #bestbuy {
            height: 250px;
        }
    }
</style>