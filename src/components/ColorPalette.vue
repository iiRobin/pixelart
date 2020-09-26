<template>
    <div id="color-palette" ref="colorPalette" style="display: none" @mousedown="handleTogglePainting(false)" @mouseup="handleTogglePainting(true)">
        <div id="color-palette-header" @mousedown="dragMouseDown">
            <span class="uk-text-bold">Select a color</span>
            <span class="close" @click.prevent="handleCloseColorPalette"><i class="fas fa-times"></i></span>
        </div>
        <div id="color-palette-container">
            <div class="pickr"></div>
        </div>
    </div>
</template>

<script>
import Pickr from '@simonwep/pickr'

export default {

    name: 'ColorPalette',

    props: [ 'selectedColor' ],

    data () {
        return {
            color: null,

            positions: {
                clientX: null,
                clientY: null,
                movementX: 0,
                movementY: 0
            },

            pickr: null,
        }
    },

    watch: {
        'color' () { this.setColor() }
    },


    created () {
        this.setColor()
    },


    mounted () {
        this.pickr = Pickr.create({
            el: '.pickr',
            theme: 'nano',
            default: this.color,
            comparison: true,
            inline: true,
            showAlways: true,

            components: {
                // Main components
                preview: true,
                hue: true,

                // Input / output Options
                interaction: {
                    input: true,
                    save: true
                }
            },
        })

        this.pickr.on('save', (color, instance) => {
            this.$emit('selectedColor', color.toHEXA().toString())
        })
    },


    methods: {

        handleTest () {
            console.log('Test')
        },

        /**
         * Set the current color.
         */
        setColor () {
            this.color = this.selectedColor
        },

        /**
         * Move the element to the mouse position.
         */
        dragMouseDown (event) {
            event.preventDefault()

            this.positions.clientX = event.clientX
            this.positions.clientY = event.clientY

            document.onmousemove = this.elementDrag
            document.onmouseup = this.closeDragElement
        },

        /**
         * Drag the current element.
         */
        elementDrag (event) {
            event.preventDefault()
            this.positions.movementX = this.positions.clientX - event.clientX
            this.positions.movementY = this.positions.clientY - event.clientY

            this.positions.clientX = event.clientX
            this.positions.clientY = event.clientY

            this.$refs.colorPalette.style.top = (this.$refs.colorPalette.offsetTop - this.positions.movementY) + 'px'
            this.$refs.colorPalette.style.left = (this.$refs.colorPalette.offsetLeft - this.positions.movementX) + 'px'
        },

        /**
         * Close the drag element.
         */
        closeDragElement () {
            document.onmouseup = null
            document.onmousemove = null
        },

        /**
         * Close the color palette.
         */
        handleCloseColorPalette () {
            $("#color-palette").hide()
        },

        /**
         * Toggle the ability to paint.
         */
        handleTogglePainting (boolean) {
            this.$emit('canPaint', boolean)
        },

    }
}
</script>

<style>
@import '../../node_modules/@simonwep/pickr/dist/themes/nano.min.css';

#color-palette {
    position: absolute;
    z-index: 9;
    text-align: center;
}
#color-palette-header {
    border: 1px solid black;
    border-bottom: none;
    padding: 10px;
    background-color: lightblue;
    text-align: left;
    cursor: move;
    z-index: 10;
    color: #1c1c1c;
}
.pcr-button {
    position: absolute !important;
    bottom: 0;
    margin-left: 4px;
    margin-bottom: 7px;
}
.close {
    float: right;
    cursor: pointer;
}
</style>
