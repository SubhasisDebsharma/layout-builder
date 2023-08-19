<script>
    /**
     * @typedef {import("../model/Colum").Column} Column
     * @typedef {import("../model/Row").Row} Row
     */

	import DragerElment from "./DragerElment.svelte";

    export let gridView = true;
    /**
     * @type Array<Row>
     */
    export let rows = [{}, {}, {}, {}, {}];
    /**
     * @type Array<Column>
     */
    export let columns = [{}, {}, {}, {}];
    let rowsDragElements = [];

</script>

<div class="root">
    {#if gridView}
        <div class="grid-view-root"
        style:grid-template-columns={columns.map(col => col.size || 'auto').join(' ')}
        style:grid-template-rows={rows.map(row => row.size || 'auto').join(' ')}
        >
            {#each rows as row, i}
                {#each columns as col, j}
                    {#if i == 0 || j == 0}
                        <div class="grid-view-item"
                        style:grid-row="{i+1} / span 1"
                        style:column-row="{j+1} / span 1"
                        >   
                            {#if i == 0}
                                <DragerElment type='column' />
                            {/if}
                            {#if j == 0}
                                <DragerElment type='row' />
                            {/if}
                        </div>
                    {/if}
                    
                {/each}
            {/each}
        </div>
    {/if}
</div>

<style>
    :global(*) {
        box-sizing: border-box;
        padding: 0;
        margin: 0;
    }

    .root {
        height: 100%;
        width: 100%;
        position: relative;
    }

    .grid-view-root {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;

        display: grid;
    }
    
    .grid-view-item {
        border: 1px dotted rgba(0, 0, 0, 0.25);
        position: relative;
    }
</style>