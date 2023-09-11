
<script>
    export let item
</script>

<div class="preset">
    <span class="title">{item.title} ({item.count})</span>
    <div style="margin: 0;">
        <button on:click={() => item.volume = true}>Hør</button>
        <button on:click={() => item.volume = false}>Sluk</button>
    </div>                                
</div>

<style>
.preset {
    border-radius: 0px;
    border-style: solid;
    border-color: #ffffff #666666 #333333 #cccccc;
    border-width: 1px;
    padding: 2px;
    margin: 0px;
    background-color: #bbbbbb;
}

.title {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 11px;
    margin: 0;
    padding: 0;
}

</style>