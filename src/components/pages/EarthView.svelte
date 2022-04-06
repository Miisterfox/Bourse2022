<script lang="ts">
    import EarthCanvas from "../molecules/EarthCanvas.svelte";
</script>

<style>
    .text-wrapper {
        position: absolute;
        display: grid;
        height: 100%;
        width: 100%;
        grid-template-columns: 5% 30% 30% 30% 5%;
        grid-template-rows: 20% 20% 20% 20% 20%;
    }
    .text{
        padding:0 10%;
        font-size:100px;
        grid-row-start: 3;
        grid-column-start:2;
    }
</style>

<div class="wrapper">
    <div class="text-wrapper">
        <div class="text">What is planet earth's state ?</div>
    </div>
    <EarthCanvas />
</div>
