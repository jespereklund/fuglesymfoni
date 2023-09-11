<script>
    export let data
</script>
<main>
    <br><br>
    <div class="table">
        {#each data as item}
            <div class="item">{item.title}</div>
            <!-- svelte-ignore a11y-missing-attribute -->
            <div class="item"><img class="img" src="images_100/{item.image}" /></div>
            <div class="item">Hør: <input type="checkbox" style="vertical-align:middle;" bind:checked={item.settings.volume}></div>
        {/each}
    </div>
</main>
<style>
    .table {
        gap: 2px;
        background-color: #999999;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr; 
        max-width: 600px;
    }

    .item {
        background-color: #dddddd;
    }
</style>