<template>
  <div
    class="rulers"
    @mousedown="setMouseDown"
    @mouseup="setMouseUp"
    @mousemove="setRulerPosition"
    @wheel="rotateRuler"
  >
    <div
      v-if="showRuler"
      class="ruler-top"
    >
      <template
        v-for="(cm, index) in rulerSize"
      >
        <div
          :key="index"
          class="cm"
        >
          <p class="cm-divisor">
            <span /><span />
          </p>
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <p
            class="cm-divisor"
            v-if="index === size - 1"
          >
            <span /><span />
          </p>
        </div>
      </template>
    </div>
    <div class="ruler-middle">
      <div
        class="ruler-size"
        v-if="!fixedSize"
      >
        <span
          class="ruler-size-sub"
          v-if="rulerSize > 3"
          @click="subtractSize"
        > - </span>
        <span class="ruler-size-value"> {{ rulerSize }} </span>
        <span
          class="ruler-size-add"
          @click="addSize"
        > + </span>
      </div>
      <div class="ruler-angle">
        <span class="ruler-angle-value">
          {{ rotation }}
        </span>
      </div>
    </div>
    <div
      v-if="showRuler"
      class="ruler-bottom"
    >
      <template
        v-for="(cm, index) in rulerSize"
      >
        <div
          :key="index"
          class="cm"
        >
          <p class="cm-divisor">
            <span /><span />
          </p>
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <div class="mm" />
          <p
            class="cm-divisor"
            v-if="index === size - 1"
          >
            <span /><span />
          </p>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Ruler',
  created () {
    this.rulerSize = this.size
  },
  props: {
    readOnly: {
      type: Boolean,
      default: false
    },
    showRuler: {
      type: Boolean,
      default: true
    },
    fixedSize: {
      type: Boolean,
      default: true
    },
    size: {
      type: Number,
      default: () => (20)
    }
  },
  data: () => ({
    mouseDown: false,
    rulerSize: 20,
    rotation: 0,
    offset: [0, 0]
  }),
  methods: {
    addSize () {
      this.rulerSize += 1
    },
    subtractSize () {
      this.rulerSize -= 1
    },
    setMouseUp () {
      this.mouseDown = false
    },
    rotateRuler ($event) {
      let value = ($event.deltaY > 0 ? $event.deltaY - 99 : $event.deltaY + 99)
      let ruler = document.querySelector('.rulers')
      let rulerAngle = document.querySelector('.ruler-angle')
      this.rotation = this.rotation + value
      if (this.rotation > 360 || this.rotation < -360) {
        this.rotation = 0
      }
      ruler.style.transform = `translate(-50%, -50%) rotate(${this.rotation}deg)`
      rulerAngle.style.transform = `translate(-50%, -50%) rotate(${this.rotation * (-1)}deg)`
    },
    setMouseDown (event) {
      this.mouseDown = true
      let ruler = document.querySelector('.rulers')
      this.offset = [
        ruler.offsetLeft - event.clientX,
        ruler.offsetTop - event.clientY
      ]
    },
    setRulerPosition (event) {
      if (!this.mouseDown) {
        return
      }
      let ruler = document.querySelector('.rulers')
      event.preventDefault()
      if (this.mouseDown) {
        let mousePosition = {
          x: event.clientX,
          y: event.clientY
        }
        ruler.style.left = (mousePosition.x + this.offset[0]) + 'px'
        ruler.style.top = (mousePosition.y + this.offset[1]) + 'px'
      }
    }
  },
  watch: {
    value: {
      handler (value) {
        this.rulerSize = value
      }
    }
  }
}
</script>

<style
  lang="stylus"
  rel="stylesheet/stylus"
  scoped
>
.rulers
  z-index 5
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column-reverse;

.ruler
  font-size: 0.6em;
  line-height: 1em;
  letter-spacing: -1px;
  height: 30px;
  text-align: left;
  font-weight: 700;
  padding: 1px 0 0 0;
  background #ddd
  opacity 0.8
  display flex
  flex-direction row
  .cm
    display flex
    flex-direction row
  .mm
    border-right: 1px solid black;
    height: 10px;
    width: 5px;
  .cm-divisor
    margin: 0;
    position: relative;
    height: 6px;
    top: -2px;
  span
    display: block;
    border-right: 1px solid black;
    height: 10px;
    width: 5px;

.ruler-top
  @extends .ruler
  transform: rotateX(180deg);

.ruler-bottom
  @extends .ruler

.ruler-middle
  opacity 0.8
  position relative
  border-style double
  border-color #76726a
  border-left  1px
  border-right 1px
  background: #ddd
  height 50px
  padding 0 10px

.ruler-angle
  position: absolute;
  top 50%
  left 50%
  transform translate(-50%, -50%)
  width: 45px;
  height: 45px;
  border 1px solid black
  border-radius 50%
  display flex
  justify-content center
  align-items center
  .ruler-angle-value
    font-size: 10px;
    font-weight: 800;
    &:after
      content: "\00b0"
      font-size: 19px;
      color: black;

.ruler-size
  position: absolute;
  top 5px
  left 20px
  display flex
  flex-direction row
  .ruler-size-add, .ruler-size-value, .ruler-size-sub
    border-radius: 3px;
    padding: 2px;
    height: 30px;
    width: 30px;
    font-size: 18px;
    border: 1px solid #000;
    font-weight: bold;
    margin: 2px;
    display: flex;
    justify-content: center;
    align-items: center;
  .ruler-size-add:hover, .ruler-size-sub:hover
    cursor pointer
    background white
</style>
