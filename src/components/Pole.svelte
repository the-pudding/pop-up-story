<script>
    import { onMount } from 'svelte';
    import inView from "../actions/inView.js";

    let y;
    let innerHeight;
    let padding = 200;
    let h;

    onMount(() => {
		h = Math.max(
            document.body.clientHeight, document.documentElement.clientHeight
        );
	});
</script>

<svelte:window bind:scrollY={y} />

<section id="pole">
    <div>
        <div class="line" style="height: {h}px;"></div>
        <div class="lil-nas-x" style="transform: translate(0,{y + padding}px)">
            <img src="assets/images/lilnasx.png">
        </div>
    </div>
</section>

<style>
    #pole {
        position: absolute;
        top: 0;
        right: 6rem;
        pointer-events: none;
    }

    div {
        position: relative;
    }

    .line {
        position: absolute;
        width: 10px;
        background-color: var(--off-black); 
    }

    .lil-nas-x {
        position: absolute;
        right: -75px;
        height: 150px;
        width: 150px;
    }
</style>