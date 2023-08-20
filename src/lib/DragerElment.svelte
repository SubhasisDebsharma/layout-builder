<script>
    /**
     * @typedef {import("../model/Event").MoveEvent} MoveEvent
     */
	import { onMount } from "svelte";

    /**
     * @type {'row' | 'column'}
     * @default 'row'
     */
    export let type = 'row';
    /**
     * @type {function(MoveEvent): void}
     */
    export let onMove;
    let x = 0;
    let y = 0;
    /**
     * @type HTMLElement
     */
    let element;

    onMount(() => {
        if (type == 'row') {
            element.style.width = (element.parentElement?.parentElement?.clientWidth || 0) + "px";
            const parentElementRect = element.parentElement?.getBoundingClientRect();
            element.style.top = ((parentElementRect?.top || 0) + (parentElementRect?.height || 0)) + 'px';
            element.style.height = "0";
            // element.style.bottom = "0";
        } else {
            element.style.width = "0";
            // element.style.right = "0"
            const parentElementRect = element.parentElement?.getBoundingClientRect();
            element.style.left = ((parentElementRect?.left || 0) + (parentElementRect?.width || 0)) + 'px';
            element.style.height = (element.parentElement?.parentElement?.clientHeight || 0) + "px";
        }

        /**
         * @param {MouseEvent} e
         */
        function handleMouseDown(e) {
            x = e.clientX;
            y = e.clientY;
            document.addEventListener('mousemove', handleMouseMove);
            document.addEventListener('mouseup', handleMouseUp);
        }

        /**
         * @param {MouseEvent} e
         */
        function handleMouseMove(e) {
            if (type == 'row') { 
                const dy = e.clientY - y;
                y = e.clientY;
                element.style.top = `${y}px`;
                onMove({dx: 0, dy, target: element});
            } else {
                const dx = e.clientX - x;
                x = e.clientX;
                element.style.left = `${x}px`;
                onMove({dx, dy: 0, target: element});
            }
        }

        /**
         * @param {MouseEvent} e
         */
        function handleMouseUp(e) {
            x = 0;
            y = 0;
            document.removeEventListener('mousemove', handleMouseMove);
            document.removeEventListener('mouseup', handleMouseUp);
        }

        element.addEventListener('mousedown', handleMouseDown);
        
        return () => {}
    })
</script>

<div bind:this={element} class="dragger" class:col-dragger={type == 'column'} class:row-dragger={type == 'row'}>

</div>

<style>
    .dragger {
        position: fixed;
        z-index: 1;
    }
    .col-dragger {
        border-right: 2px solid red;
        cursor: col-resize;
    }
    .row-dragger {
        border-bottom: 2px solid red;
        cursor: row-resize;
    }
</style>