<script>
    export let data
</script>
<main>
    <div class="grid">
        {#each data as item}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <div on:click={() => item.settings.volume = !item.settings.volume} class="item" style="background-color: {item.settings.volume > 0 ? '#ffffff' : '#cccccc'}; cursor: pointer;">
                <p class="title">{item.title}</p>
                <div class="box">
                    <div class="inner-box">
                        <!-- svelte-ignore a11y-missing-attribute -->
                        <img class="img" src="images_100/{item.image}" />
                    </div>
                </div>
            </div>
        {/each}
    </div>
</main>

<style>
.grid {
    display: flex;
    flex-flow: row wrap;    
    gap: 2px;
}

.item {
    border-radius: 5px;
    border-style: solid;
    border-color: #ffffff #888888 #666666 #cccccc;
    border-width: 1px;
    text-align: center;
    background-color: #bbbbbb;
    width: 140px;
    height: 150px;
    overflow-wrap: break-word;
    padding: 2px;
}

.title {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 11px;
    margin: 0px;
}

.box {
  width: 120px;
  height: 120px;
  display: table;
  margin-left: 8px;
  margin-top: 4px;
  margin-bottom: 2px;
  padding-top: 0px;
}

.inner-box{
  text-align:center;
  display:table-cell;
  vertical-align: middle;
}

.img {
    object-fit: contain;        
    vertical-align: middle;
    border-style: solid;
    border-width: 0px;
    width: 120px;
    height: 120px;

}
</style>