<script>
    export let left = 0;
    export let top = 0;
    let moving = false;
    let movement = null;

    function startDrag() { moving = true; }
    function stopDrag() { moving = false; }

    function moveDrag(e) {

        if (moving) {
            left += e.movementX;
            console.log(left, e.movementX)

            if (left < 0) {
                left = 0
            }
        }
    }
</script>

<svelte:window on:mouseup={stopDrag} on:mousemove={moveDrag}/>

<section on:mousedown={startDrag} style="left: {left}px; top: {top}px;" class="draggable">
    <slot></slot>
</section>

<style>
    .draggable {
        user-select: none;
        position: relative;
        cursor: move;
        width: 100%;
        height: 100%;
        border-radius: 0 0.25rem 0.25rem 0;
        background-color: #f4f4f4;
    }
</style>
  
