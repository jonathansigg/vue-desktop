<template>
  <div :id="`window-${name}`" v-bind:class="{ full: full, window: true }" :style="`width:${width}px; height:${height}px;top:${y}px;left:${x}px;z-index:${z}`">
    <div @mousedown="startDrag" class="window-header">
      <div class="window-name">{{ name }}</div>
      <div class="window-btn-wrapper">
          <button v-if="minbtn" v-on:click="min" class="window-btn btn-mini"></button>
          <button v-if="fullbtn" v-on:click="big" class="window-btn btn-big"></button>
          <button v-if="closebtn" v-on:click="close" class="window-btn btn-close"></button>
        </div>
    </div>
    <div class="window-content">
      <myconsole @close="close" v-if="component == 'console'"></myconsole>
      <options v-if="component == 'options'"></options>
    </div>
  </div>
</template>

<script>
import myconsole from './windows/Console.vue'
import options from './windows/Options.vue'
import emitter from 'tiny-emitter/instance'

export default {
  components: {
    myconsole,
    options
  },
  props: {
    name: { default: 'window', type: String },
    component: { default: '', type: String },
    hash: { default: '', type: String },
    closebtn: { default: true, type: Boolean },
    minbtn: { default: true, type: Boolean },
    fullbtn: { default: true, type: Boolean }
  },
  data: () => {
    return {
      dragging: false,
      x: 100,
      y: 50,
      z: 2,
      width: 800,
      height: 600,
      defaultZ: 1,
      startX: 0,
      startY: 0,
      prevX: 0,
      prevY: 0,
      full: false
    }
  },
  mounted() {
    const self = this;
    window.addEventListener('mouseup', function(){
      self.stopDrag()
    })
    window.addEventListener('mousemove', function(e){
      self.doDrag(e)
    })

    emitter.emit('resetZ')

    emitter.on('resetZ', function() {
      self.z = self.defaultZ
    })
  },
  methods: {
    min: function() {

    },
    big: function() {
      if(this.full == false) {
        this.full = true
      } else {
        this.full = false
      }
    },
    close: function() {
      emitter.emit('close-window', this.hash)
    },
    startDrag(event) {
      this.dragging = true
      this.startX = this.x
      this.startY = this.y
      this.prevX = event.screenX
      this.prevY = event.screenY
      emitter.emit('resetZ')
      this.z = 2
    },
    stopDrag() {
      this.dragging = false;
      this.width = parseInt(this.$el.style.width)
      this.height = parseInt(this.$el.style.height)
    },
    doDrag(event) {
      if(this.dragging) {
        if(this.full) {
          this.full = false
          this.startX = event.screenX - (this.width / 2)
          this.startY = event.screenY - 110
        }
        this.x = this.startX + (event.screenX - this.prevX)
        this.y = this.startY + (event.screenY - this.prevY)
      }
    }
  }
}
</script>

<style>

.btn-mini {
  background: #ffc34d;
}

.btn-big {
  background: #41d627;
}

.btn-close {
  background: #d62727;
}

.window {
  position: absolute;
  width: 800px;
  overflow: hidden;
  border-radius: 5px;
  resize: both;
  min-width: 200px;
  min-height: 200px;
}

.window.full {
  width: 100% !important;
  height: calc(100% - 45px) !important;
  top: 0 !important;
  left: 0 !important;
  z-index: 10 !important;
  border-radius: 0;
}

.window .window-header {
  height: 25px;
  width: 100%;
  background: #DAD9D9;
  z-index: 10;
  display: flex;
  justify-content: space-between;
}

.window .window-name {
  display: flex;
  align-items: center;
  padding: 0 0.5rem;
  font-size: 0.7rem;
}

.window .window-btn {
  padding: 5px;
  margin: 0 3px;
  cursor: pointer;
  border: none;
  height: 10px;
  width: 10px;
  border-radius: 100%;
  outline: none;
}

.window .window-btn-wrapper {
  display: flex;
  align-items: center;
  padding-right: 0.2rem;
}

.window .window-content {
  width: 100%;
  height: calc(100% - 25px);
  background: var(--grey);
  padding: 1rem;
  margin: 0;
}
</style>