<script>
    /**
     * @type {'row' | 'column'}
     * @default 'row'
     */
    export let type = 'row';
    /**
     * @type HTMLElement
     */
    let element;
    $: {
        if (element) {
            if (type == 'row') {
                element.style.width = (element.parentElement?.parentElement?.clientWidth || 0) + 10 + "px";
                element.style.height = "100%";
                element.style.resize = "vertical";
            } else {
                element.style.width = "100%";
                element.style.height = (element.parentElement?.parentElement?.clientHeight || 0) + 10 + "px";
                element.style.resize = "horizontal"
            }
        }
    }
</script>
<div bind:this={element} class="dragger">

</div>

<style>
    .dragger {
        border: 1px solid red;
        position: absolute;
        overflow: auto;
    }
</style>