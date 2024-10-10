<template>
  <div :style="{ cursor, userSelect}" class="vue-splitter-container" @mouseup="onMouseUp" @mousemove="onMouseMove">
    <div class="vue-splitter-flex-box" :split="split" :class="split">
      <pane class="splitter-pane splitter-paneL" :style="{ [type]: width? width + 'px' :  (percent+'%')}">
        <slot name="paneL"></slot>
      </pane>

      <pane class="splitter-pane splitter-paneR" :style="{ [type]: width? 'calc(100% - ' + width + 'px)'  :(100-percent+'%')}" >
        <slot name="paneR"></slot>
      </pane>
    </div>

    <resizer :className="className" :style="{ [resizeType]:width? width +'px' : percent+'%'}" :split="split" @mousedown.native="onMouseDown" @click.native="onClick"></resizer>

    <div class="vue-splitter-container-mask" v-if="active"></div>
  </div>
</template>

<script>
  import Resizer from './resizer.vue'
  import Pane from './pane.vue'

  export default {
    name: 'splitPane',
    components: { Resizer, Pane },
    props: {
      defaultPx :{
        type: Number,
        default: 0,
        required: false
      },
      minPx:{
        type: Number,
        default: 300
      },
      minPercent: {
        type: Number,
        default: 10
      },
      defaultPercent: {
        type: Number,
        default: 50
      },
      split: {
        validator(value) {
          return ['vertical', 'horizontal'].indexOf(value) >= 0
        },
        default:'vertical'
      },
      className: String
    },
    computed: {
      userSelect() {
        return this.active ? 'none' : ''
      },
      cursor() {
        return this.active ? (this.split === 'vertical' ? 'col-resize' : 'row-resize') : ''
      }
    },
    watch: {
      defaultPercent(newValue,oldValue){
        this.percent = newValue
      },
      defaultPx(newValue,oldValue){
        this.width = newValue
      }
    },
    data() {
      return {
        active: false,
        hasMoved: false,
        height: null,
        percent: this.defaultPercent,
        type: this.split === 'vertical' ? 'width' : 'height',
        resizeType: this.split === 'vertical' ? 'left' : 'top',
        width: this.defaultPx,
      }
    },
    methods: {
      onClick() {
        if (!this.hasMoved) {
          this.percent = 50
          this.$emit('resize', this.percent)
        }
      },
      onMouseDown() {
        this.active = true
        this.hasMoved = false
      },
      onMouseUp() {
        this.active = false
      },
      onMouseMove(e) {
        if (e.buttons === 0 || e.which === 0) {
          this.active = false
        }

        if (this.active) {
          let offset = 0
          let target = e.currentTarget
          if (this.split === 'vertical') {
            while (target) {
              offset += target.offsetLeft
              target = target.offsetParent
            }
          } else {
            while (target) {
              offset += target.offsetTop
              target = target.offsetParent
            }
          }

          const currentPage = this.split === 'vertical' ? e.pageX : e.pageY
          const targetOffset = this.split === 'vertical' ? e.currentTarget.offsetWidth : e.currentTarget.offsetHeight
          const width = currentPage - offset

          if (this.defaultPx) {
            if (width > this.minPx && width < targetOffset - this.minPx) {
              this.width = width
            }
            this.$emit('resize', this.width)
          }else{
            const percent = Math.floor((width / targetOffset) * 10000) / 100
            if (percent > this.minPercent && percent < 100 - this.minPercent) {
              this.percent = percent
            }
            this.$emit('resize', this.percent)
          }
          this.hasMoved = true
        }
      }
    }
  }
</script>

<style scoped>
.vue-splitter-container {
  height: 100%;
  position: relative;
}

.vue-splitter-flex-box{
  display: flex;
  height:inherit;
  width:inherit;
}

.vue-splitter-flex-box.horizontal{
  flex-direction: column ;
}

.vue-splitter-container-mask {
  z-index: 9999;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}
</style>