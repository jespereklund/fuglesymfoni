<script>
    import GroupItem from "./GroupItem.svelte";
    export let data
</script>
<div>
    <div class="grid">
    {#each data as item}
        <GroupItem bind:item={item} />
    {/each}
    </div>
</div>
<style>
.grid {
    display: flex;
    flex-flow: row wrap;    
    gap: 2px;
}
</style>