<template>
  <canvas :id="id" class="f2-canvas" :width="width" :height="height" @touchStart="touchStart" @touchMove="touchMove" @touchEnd="touchEnd" />
</template>

<script>
import F2 from '@antv/f2'
// #ifdef MP-WEIXIN
import { wx as F2Context } from '@antv/f2-context'
// #endif
// #ifdef MP-ALIPAY
import { my as F2Context } from '@antv/f2-context'
// #endif

let f2Id = 0

function wrapEvent(e) {
  if (!e) return
  if (!e.preventDefault) {
    e.preventDefault = function() {}
  }
  return e
}

export default {
  data() {
    return {
      id: '',
      width: 0,
      height: 0,
    }
  },
  created() {
    this.id = 'f2-canvas-' + ++f2Id
  },
  mounted() {
    const myCtx = uni.createCanvasContext(this.id)
    const context = F2Context(myCtx)
    const query = uni.createSelectorQuery()
    query
      .select('#' + this.id)
      .boundingClientRect()
      .exec(res => {
        // 获取画布实际宽高
        const { width, height } = res[0]
        const pixelRatio = uni.getSystemInfoSync().pixelRatio
        this.width = width * pixelRatio
        this.height = height * pixelRatio
        this.$nextTick(() => {
		  const chart = new F2.Chart({ context, width, height, pixelRatio })
          if (chart) {
            this.chart = chart;
            this.canvasEl = chart.get('el');
          }
		  this.$emit('init', chart);
        })
      })
  },
  methods: {
    touchStart(e) {
      const canvasEl = this.canvasEl
      if (!canvasEl) {
        return
      }
      canvasEl.dispatchEvent('touchstart', wrapEvent(e))
	  // console.log(e);
	  // this.$emit('touchStart', e);
    },
    touchMove(e) {
      const canvasEl = this.canvasEl
      if (!canvasEl) {
        return
      }
      canvasEl.dispatchEvent('touchmove', wrapEvent(e))
	  // this.$emit('touchMove', e);
    },
    touchEnd(e) {
      const canvasEl = this.canvasEl
      if (!canvasEl) {
        return
      }
      canvasEl.dispatchEvent('touchend', wrapEvent(e))
    }
  }
}
</script>

<style scoped>
.f2-canvas {
  width: 100%;
  height: 100%;
}
</style>
