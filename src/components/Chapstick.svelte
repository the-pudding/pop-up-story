<script>
    import Draggable from "./Draggable.svelte";
</script>
  
<section class="chapstick">
    <div class="tube">
        <div class="inner-label">
            <p>ChapStick</p>
        </div>
    </div>
    <div class="top">
        <Draggable>
        </Draggable> 
    </div>
</section>

<style>
    .chapstick {
        margin: 2rem;
        width: 300px;
        display: flex;
        flex-direction: row;
        height: 75px;
    }

    .tube {
        width: 220px;
        height: 75px;
        background-color: #BB195B;
        border-radius: 0.25rem 0 0 0.25rem;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .inner-label {
        background-color: #282828;
        width: 90%;
        height: 50%;
        border: 3px solid gray;
        border-radius: 0.25rem;
    }

    .inner-label p {
        text-align: center;
        padding: 0;
        margin: 0;
        color: white;
        font-family: cursive;
        font-size: 1.5rem;
    }

    .top {
        width: 75px;
        height: 75px;
    }
</style>
