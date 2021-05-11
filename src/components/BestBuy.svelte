<script>
</script>

<section id="bestbuy">
    <div class="left-scene">
        <div class="flag"></div>
        <div class="dvd off-screen"></div>
        <div class="building off-screen"></div>
    </div>
    <div class="right-scene">
        <div class="car"></div>
    </div>
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
        background-image: url("assets/images/bestbuy.png");
        width: 400px;
        height: 400px;
        background-size: contain;
        background-repeat: no-repeat;
        transition-duration: 1s; 
    }

    .dvd {
        position: absolute;
        background-image: url("assets/images/dvd.jpg");
        width: 100px;
        height: 100px;
        background-size: contain;
        background-repeat: no-repeat;
        transition-duration: 1s;  
        transition-delay: 0.5s;
    }

    .building.off-screen {
        transform: translate(-100%, 0);
    }

    .building.on-screen {
        transform: translate(0, 0);
    }

    .dvd.off-screen {
        transform: translate(-100%, 0);
        top: 100px;
    }

    .dvd.on-screen {
        top: 0px;
        transform: translate(200px, 0);
    }
</style>