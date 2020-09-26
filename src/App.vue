<template>
    <div id="app">
        <div style="display: flex">
            <div class="uk-sidebar">
                <ul class="uk-list uk-padding-small">
                    <li>
                        <a href :class="{ 'active': tool == 'pencil' }" @click.prevent="handleDraw" uk-tooltip="Draws lines or singular squares"><i class="fas fa-pencil-alt"></i></a>
                    </li>
                    <li>
                        <a href :class="{ 'active': tool == 'bucket' }" @click.prevent="handleFill" uk-tooltip="Allows you to paint a whole section"><i class="fas fa-fill-drip"></i></a>
                    </li>
                    <li>
                        <a href :class="{ 'active': tool == 'eraser' }" @click.prevent="handleErase" uk-tooltip="Allows you to erase lines or singular squares"><i class="fas fa-eraser"></i></a>
                    </li>
                    <li>
                        <a href @click.prevent="handleRemoveAll" uk-tooltip="Removes all the painted squares"><i class="fas fa-trash-alt"></i></a>
                    </li>
                    <li>
                        <a href @click.prevent="handleOpenColorPalette" uk-tooltip="Opens the color palette"><i class="fas fa-palette"></i></a>
                    </li>
                    <li>
                        <a href :class="{ 'active': tool == 'pipette' }" @click.prevent="handlePipette" uk-tooltip="Allows you to recover a color"><i class="fas fa-eye-dropper"></i></a>
                    </li>
                </ul>
            </div>
            <ColorPalette :selectedColor="color" @selectedColor="handleSelectColor" @canPaint="handleCanPaint" />
            <Grid :color="color" :tool="tool" :canPaint="canPaint" @color="handleSelectColor" @tool="handleSelectTool" />
        </div>
    </div>
</template>

<script>
import Grid from './components/Grid'
import ColorPalette from './components/ColorPalette'

export default {

    name: 'App',

    components: { Grid, ColorPalette },

    data () {
        return {
            color: '#000000',
            tool: null,
            canPaint: true,
        }
    },


    mounted () {
        this.handleDraw()
    },


    methods: {

        /**
         * Select the pencil tool.
         */
        handleDraw () {
            this.tool = 'pencil'
        },

        /**
         * Select the paint bucket tool.
         */
        handleFill () {
            this.tool = 'bucket'
        },

        /**
         * Select the eraser tool.
         */
        handleErase () {
            this.tool = 'eraser'
        },

        /**
         * Select the pipette tool.
         */
        handlePipette () {
            this.tool = 'pipette'
        },

        /**
         * Remove everything that has been painted.
         */
        handleRemoveAll () {
            $(".cell").css('background-color', '').css('border', '')
        },

        /**
         * Open the color palette.
         */
        handleOpenColorPalette () {
            $("#color-palette").show()
        },

        /**
         *
         */
        handleCanPaint (value) {
            this.canPaint = value
        },

        /**
         * Select a new color from the color palette.
         */
        handleSelectColor (value) {
            this.color = value
        },

        /**
         * Select a new tool.
         */
        handleSelectTool (value) {
            this.tool = value
        },

    },

}
</script>

<style>
html, body, #app {
    height: 100%;
    margin: 0;
    padding: 0;
    overflow-x: hidden;
    overflow-y: hidden;
}
.uk-sidebar {
    width: 45px;
    background-color: lightblue;
}
.fas {
    color: #1c1c1c;
}
.active i {
    color: #949494 !important;
}
</style>
