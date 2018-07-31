<template>
   <div class="resize-parent" @mousemove="mouseMove" @mouseup.left="dragStop" :style="{gridTemplateColumns: getGridTemplateColumnsCss}">
       <div class="resize-pane resize-left-pane resize-fill" ref="leftPane">
           <slot name="left"></slot>
       </div>
       <div class="resize-grab-bar resize-fill" @mousedown.left="dragStart" ref="grabBar">
           <div class="resize-grab-icon" :style="{top: mouseY}">
               <slot name="grab-icon">
                   <svg aria-hidden="true" data-prefix="fal" data-icon="ellipsis-v" class="svg-inline--fa fa-ellipsis-v fa-w-2" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 512"><path fill="currentColor" d="M32 224c17.7 0 32 14.3 32 32s-14.3 32-32 32-32-14.3-32-32 14.3-32 32-32zM0 136c0 17.7 14.3 32 32 32s32-14.3 32-32-14.3-32-32-32-32 14.3-32 32zm0 240c0 17.7 14.3 32 32 32s32-14.3 32-32-14.3-32-32-32-32 14.3-32 32z"></path></svg>
               </slot>
           </div>
       </div>
       <div class="resize-pane resize-right-pane resize-fill" ref="rightPane">
           <slot name="right"></slot>
       </div>
   </div>
</template>

<script>
    export default {
        name: "ResizePanes",
        data: function() {
           return {
               leftWidth: null,
               dragging: false,
               mouseY: 0,
           }
        },
        computed: {
            getGridTemplateColumnsCss: function() {
                return `[first left-start] ${this.leftWidth} [left-end grab-start] 15px [grab-end right-start] auto [right-end last]`;
            }
        },
        methods: {
            dragStart: function() {
                // console.log('dragStart');
                this.dragging = true;
            },
            dragStop: function() {
                // console.log('dragStop');
                this.dragging = false;
            },
            mouseMove: function(e) {
                if (this.dragging) {
                    // console.log('move');
                    let delta = e.movementX;
                    let left = this.$refs['leftPane'];

                    this.leftWidth = left.clientWidth + delta + 'px';
                    this.$emit('widthChange');
                }
                this.mouseY = e.offsetY - 15 + 'px';
            },
        },
    }
</script>

<style lang="scss" scoped>
    * {
        box-sizing: border-box;
    }
    .resize-parent {
        display: grid;
        grid-template-columns: [first left-start] 25% [left-end grab-start] 15px [grab-end right-start] auto [right-end last];
        grid-template-rows: auto;
        grid-template-areas: "left grab right";
        justify-items: start;
        align-items: start;
        justify-content: stretch;
        align-content: stretch;
        height: calc(100vh - 20px);
        width: 100%;
        max-height: calc(100vh - 20px);
        max-width: 100%;
    }
    .resize-pane {
        position: relative;
    }
    .resize-left-pane {
        grid-area: left;
    }
    .resize-grab-bar {
        grid-area: grab;
        cursor: ew-resize;
        align-self: center;
        position: relative;
        text-align: center;
        user-select: none;

        &:hover .resize-grab-icon {
            opacity: 1;
            transition: opacity 0.0s;
        }
    }
    .resize-right-pane {
        grid-area: right;
    }
    .resize-fill {
        height: 100%;
        width: 100%;
    }
    .resize-grab-icon {
        position: absolute;
        left: 0;
        right: 0;
        margin: 0 auto;
        pointer-events: none;
        opacity: 0;
        transition: opacity 0.5s;
    }
    svg.fa-ellipsis-v {
        height: 35px;
    }
</style>