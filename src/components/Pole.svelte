<script>
    import { onMount } from 'svelte';

    let y;
    let padPos = 200;
    let h;

    onMount(() => {
        setInterval(() => {
            h = Math.max(
                document.body.clientHeight, 
                document.body.scrollHeight,
                document.body.offsetHeight
            );
        }, 2000)
	});
</script>

<svelte:window bind:scrollY={y} />

<section id="pole">
    <div>
        <div class="line" style="height: {h}px;"></div>
        <div class="lil-nas-x" style="transform: translate(0,{y + padPos}px)"></div>
    </div>
</section>

<style>
    #pole {
        position: absolute;
        top: 0;
        right: 6rem;
        pointer-events: none;
        z-index: 100;
    }

    div {
        position: relative;
    }

    .line {
        position: absolute;
        width: 10px;
        background-image: linear-gradient(90deg, var(--gray), var(--gray-light), var(--gray), var(--gray-dark), var(--gray), var(--gray-light), var(--gray));
    }

    .lil-nas-x {
        position: absolute;
        background: url("../assets/images/lilnasx_drawing.png") 0 0;
        background-size: cover;
        right: -78px;
        top: 0;
        height: 150px;
        width: 150px;
        animation: poleCycle 2s steps(3) infinite;
    }

    .with-animation {
        animation: poleCycle 2s steps(3) infinite;
    }

    @keyframes poleCycle {
        100%   { background-position: 0 -450px ;}
    }

    @media only screen and (max-width: 700px) {
        #pole {
            right: 4rem;
        }

        .line {
            width: 7px;
        }

        .lil-nas-x {
            right: -65px;
            height: 125px;
            width: 125px;
        }

        @keyframes poleCycle {
            100%   { background-position: 0 -375px ;}
        }
    }

    @media only screen and (max-width: 400px) {
        #pole {
            right: 3rem;
        }

        .line {
            width: 5px;
        }

        .lil-nas-x {
            right: -50px;
            height: 100px;
            width: 100px;
        }

        @keyframes poleCycle {
            100%   { background-position: 0 -300px ;}
        }
    }
</style>