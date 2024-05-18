<template>
  <div class="width-auto uk-padding-remove">
      <div class="grid">
          <div v-for="h_item in width" :key="h_item">
              <div :id="'h-cell-'+ h_item" class="cell" @mousedown.prevent="handleCellClick" @mouseover="handleCellClick"></div>
              <div v-for="v_item in height" :key="v_item" :id="'v-cell-'+ v_item" class="cell" @mousedown.prevent="handleCellClick" @mouseover="handleCellClick"></div>
          </div>
      </div>
  </div>
</template>

<script>
export default {

    name: 'Grid',

    props: [ 'color', 'tool', 'canPaint' ],

    data () {
        return {
            width: null,
            height: null,
        }
    },


    created () {
        this.calculateWidth()
        this.calculateHeight()
    },


    mounted () {
        this.handleShortcut()

        $(window).resize(() => {
            this.calculateWidth()
            this.calculateHeight()
        })
    },


    methods: {

        /**
         * Calculcate the width of the grid.
         */
        calculateWidth () {
            this.width = Math.ceil($(window).width() / 27.5)
        },

        /**
         * Calculate the height of the grid.
         */
        calculateHeight () {
            this.height = Math.ceil($(window).height() / 28)
        },

        /**
         * What to do when clicked on a cell.
         */
        handleCellClick (event) {
            if(this.canPaint) {
                if(this.tool == 'pencil' && event.buttons == 1) {
                    $(event.target).css('background-color', this.color).css('border', '1px solid '+ this.color)
                }

                if(this.tool == 'bucket') {
                    $(".cell").each((index, cell) => {
                        if($(cell).css('background-color') == 'rgba(0, 0, 0, 0)')
                            $(cell).css('background-color', this.color).css('border', '1px solid '+ this.color)
                    })
                }

                if(this.tool == 'eraser' && event.buttons == 1) {
                    $(event.target).css('background-color', '').css('border', '')
                }

                if(this.tool == 'pipette' && event.type === 'mousedown') {
                    this.$emit('color', $(event.target).css('background-color'))
                }
            }
        },

        /**
         * Handle shortcuts.
         */
        handleShortcut () {
            let ctrlPressed = false
            let previousTool = null

            window.addEventListener('keydown', (e) => {
                if (e.key == 'Control' && !ctrlPressed) {
                    ctrlPressed = !ctrlPressed
                    previousTool = this.tool

                    this.$emit('tool', 'eraser')
                }
            })

            window.addEventListener('keyup', (e) => {
                if(e.key == 'Control') {
                    ctrlPressed = !ctrlPressed

                    this.$emit('tool', previousTool)
                }
            })
        }

    }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.width-auto {
    width: auto;
    box-sizing: border-box;
    max-width: 100%;
}
.grid {
    display: flex;
    align-items: flex-end;
    justify-content: left;
}
.cell {
    flex: 1;
    height: 25px;
    width: 25px;
    border: 1px solid lightblue;
}
</style>
