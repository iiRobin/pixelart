<template>
  <div class="width-auto uk-padding-remove">
      <div class="grid">
          <div class="horizontal" v-for="h_item in width" :key="h_item">
              <div :id="'h-cell-'+ h_item" class="cell" @mousedown.prevent="handleCellClick"></div>
              <div class="vertical" v-for="v_item in height" :key="v_item">
                  <div :id="'v-cell-'+ v_item" class="cell" @mousedown.prevent="handleCellClick"></div>
              </div>
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
            width: 56,
            height: 25,
        }
    },


    mounted () {
        document.querySelectorAll(".cell").forEach((cell) => {
            cell.addEventListener('mouseover', (e) => {
                if(e.buttons == 1) {
                    this.handleCellClick(e)
                }
            })
        })

        this.handleShortcut()
    },

    methods: {

        /**
         *
         */
        handleBlur () {
            console.log('Blur')
        },

        /**
         *
         */
        handleFocus () {
            console.log('Focus')
        },

        /**
         * What to do when clicked on a cell.
         */
        handleCellClick (event) {
            if(this.canPaint) {
                if(this.tool == 'pencil') {
                    $(event.target).css('background-color', this.color).css('border', '1px solid '+ this.color)
                }

                if(this.tool == 'bucket') {
                    $(".cell").each((index, cell) => {
                        if($(cell).css('background-color') == 'rgba(0, 0, 0, 0)')
                            $(cell).css('background-color', this.color).css('border', '1px solid '+ this.color)
                    })
                }

                if(this.tool == 'eraser') {
                    $(event.target).css('background-color', '').css('border', '')
                }

                if(this.tool == 'pipette') {
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
.horizontal {
    width: 26px;
    float: left;
}
.cell {
    height: 25px;
    width: 25px;
    border: 1px solid lightblue;
}
</style>
