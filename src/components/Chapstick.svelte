<script>
    import { onMount } from 'svelte';
    import {select, selectAll} from "d3";
    import inView from "../actions/inView.js";

    let y;
    let innerHeight;
    let padding = 200;
    let h;
    let triggerChapstick;

    onMount(() => {
		h = Math.max(
            document.body.clientHeight, 
            document.body.scrollHeight,
            document.body.offsetHeight
        );
	});
</script>

<section id="chapstick" style="height: {h}px;">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</section>

<style>
    #chapstick {
        position: absolute;
        pointer-events: none;
        width: 100%;
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
        display: none;
        overflow-x: hidden;
    }

    div { 
        width: 200px;
        height: 63px;
        background-image: url("assets/images/chapstick.png");
        background-repeat: no-repeat;
        background-size: contain;
        opacity: 0;
    }
</style>