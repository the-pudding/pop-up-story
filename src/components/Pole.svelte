<script>
    import { onMount } from 'svelte';
    import { fly } from "svelte/transition";

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
        <div class="lil-nas-x">
            <div transition:fly="{{ y: -200, duration: 1000 }}" class="bubble">
                <p>"One day when somebody says this, it's just gonna be 'Oh that person said this' and not even think about it." <span>— Lil Nas X</span></p>
            </div>
        </div>
    </div>
</section>

<style>
    #pole {
        position: fixed;
        top: 0;
        right: 6rem;
        pointer-events: none;
        z-index: 100;
        height: 100vh;
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
        top: -400px;
        height: 150px;
        width: 150px;
        transition: top 1s ease-out;
    }

    .bubble {
        padding: 1rem;
        background-color: rgba(255, 255, 255, 0.5);
        position: absolute;
        left: -100px;
        width: 100%;
        border-radius: 0.25rem;
        opacity: 0;
        transition: opacity 1s linear;
    }

    .bubble p {
        font-size: 0.75rem;
        margin: 0;
    }

    .bubble span {
        font-weight: 700;
    }

    .bubble:after {
        content: '';
        position: absolute;
        right: 0;
        top: 50%;
        width: 0;
        height: 0;
        border: 20px solid transparent;
        border-left-color: rgba(255, 255, 255, 0.5);
        border-right: 0;
        margin-top: -20px;
        margin-right: -20px;
    }

    @keyframes poleCycle {
        100%   { background-position: 0 -450px ;}
    }

    .with-animation {
        animation: poleCycle 2s steps(3) infinite;
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

        .with-animation {
            animation: poleCycle 2s steps(3) infinite;
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

        .with-animation {
            animation: poleCycle 2s steps(3) infinite;
        }
    }
</style>