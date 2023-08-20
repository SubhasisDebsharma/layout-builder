<script>
    /**
     * @typedef {import("../model/Colum").Column} Column
     * @typedef {import("../model/Row").Row} Row
     * @typedef {import("../model/Event").MoveEvent} MoveEvent
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

    /**
     * 
     * @param {MoveEvent} e
     * @param {number} i
     * @param {number} j
     */
    function handleMove(e, i, j) {
        const element = e.target.parentElement;
        
        if (e.dx) {
            const borderWidth = 2;
            const currentElementWidth = element?.clientWidth || 0;
            const nextElementWidth = element?.nextElementSibling?.clientWidth || 0;
            const currentElementNewWidth = (currentElementWidth + e.dx + borderWidth);
            const totalWidth = currentElementWidth + nextElementWidth + borderWidth*2;
            columns[j].size = currentElementNewWidth + "px";
            const nextElementNewWidth = totalWidth - currentElementNewWidth;
            columns[j+1].size = nextElementNewWidth + "px";
        } else {
            const borderWidth = 2;
            const currentElementWidth = element?.clientHeight || 0;
            /**
             * @type any
             */
            let nextElementSibling = element?.nextElementSibling;
            while(element?.dataset?.columnIndex !== nextElementSibling?.dataset?.columnIndex) {
                nextElementSibling = nextElementSibling?.nextElementSibling
            }

            const nextElementWidth = nextElementSibling?.clientHeight || 0;
            const currentElementNewWidth = (currentElementWidth + e.dy + borderWidth);
            const totalWidth = currentElementWidth + nextElementWidth + borderWidth*2;
            rows[i].size = currentElementNewWidth + "px";
            const nextElementNewWidth = totalWidth - currentElementNewWidth;
            rows[i+1].size = nextElementNewWidth + "px";
        }
    }
    /**
     * @type HTMLElement
     */
    let gridViewRootElement

    let defaultSizeCol = (100 / columns.length) + '%';
    let defaultSizeRow = (100 / rows.length) + '%';

</script>

<div class="root">
    {#if gridView}
        <div class="grid-view-root"
        bind:this={gridViewRootElement}
        style:grid-template-columns={columns.map(col => col.size || defaultSizeCol).join(' ')}
        style:grid-template-rows={rows.map(row => row.size || defaultSizeRow).join(' ')}
        >
            {#each rows as row, i}
                {#each columns as col, j}
                        <div class="grid-view-item"
                        style:grid-row="{i+1} / span 1"
                        style:column-row="{j+1} / span 1"
                        data-row-index={i}
                        data-column-index={j}
                        >
                            {#if i == 0 && j!=(columns.length-1)}
                                <DragerElment type='column' onMove={(e) => handleMove(e, i, j)}  />
                            {/if}
                            {#if j == 0 && i!=(rows.length-1)}
                                <DragerElment type='row' onMove={(e) => handleMove(e, i, j)} />
                            {/if}
                        </div>
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
        border: 2px solid red;
    }
    
    .grid-view-item {
        border: 1px dotted rgba(0, 0, 0, 0.25);
        position: relative;
    }
</style>