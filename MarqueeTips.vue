<template>
  <div class="marquee-tips">
    <slot name="icon"></slot>
    <div class="marquee-tips-container"
      @mouseenter="handleMouseEnter"
      @mouseleave="handleMouseLeave">
      <div ref="marqueeBox" :style="boxStyle">
        <i ref="marqueeText">
          <slot></slot>
        </i>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'MarqueeTips',
    data () {
      return {
        translateX: 0,
        textWidth: 0,
        destroy: false
      }
    },
    props: {
      speed: {
        type: Number,
        default: 1
      },
      list: Array,
      moving: {
        type: Boolean,
        default: true
      }
    },
    methods: {
      initTextWidth () {
        this.textWidth = this.$refs.marqueeText.offsetWidth
      },
      move () {
        if (this.destroy || !this.moving) return

        this.translateX -= this.speed
        if (this.translateX < -this.textWidth) {
          this.translateX = this.$refs.marqueeBox.offsetWidth
        }
        window.requestAnimationFrame(this.move)
      },
      handleMouseEnter () {
        this.$emit('mouseenter')
      },
      handleMouseLeave () {
        this.$emit('mouseleave')
      }
    },
    computed: {
      boxStyle () {
        return `transform: translate3d(${this.translateX}px, 0, 0)`
      }
    },
    watch: {
      moving (to, from) {
        if (to && !from) {
          this.move()
        }
      },
      update () {
        this.initTextWidth()
      }
    },
    mounted () {
      this.initTextWidth()
      this.move()
    },
    destroyed () {
      this.destroy = true
    }
  }
</script>

<style>
  .marquee-tips{
    display: flex;
    margin-bottom: 20px;
    font-size: 14px;
    line-height: 16px;
    color: #FF8447;
  }

  .marquee-tips-container{
    flex: 1;
    overflow: hidden;
    white-space: nowrap;
  }
</style>
