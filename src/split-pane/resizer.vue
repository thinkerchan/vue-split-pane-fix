<template>
	<div :class="classes"></div>
</template>

<script>
  export default {
    props: {
      split: {
        validator(value) {
          return ['vertical', 'horizontal'].indexOf(value) >= 0
        },
        required: true
      },
      className: String
    },
    computed: {
      classes() {
        const classes = ['splitter-pane-resizer', this.split, this.className]
        return classes.join(' ')
      }
    }
  }
</script>

<style scoped>
.splitter-pane-resizer {
	box-sizing: border-box;
	background: #000;
	position: absolute;
	opacity: .2;
	z-index: 1;
	background-clip: padding-box;
}

.splitter-pane-resizer.horizontal {
  height: 11px;
	margin: -5px 0;
	border-top: 5px solid rgba(255, 255, 255, 0);
	border-bottom: 5px solid rgba(255, 255, 255, 0);
	cursor: row-resize;
	width: 100%;
}

.splitter-pane-resizer.vertical {
  width: 11px;
	height: 100%;
	margin-left: -5px;
	border-left: 5px solid rgba(255, 255, 255, 0);
	border-right: 5px solid rgba(255, 255, 255, 0);
	cursor: col-resize;
  top: 0;
}
</style>
