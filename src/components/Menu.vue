<template>
  <div id="menu" v-bind:class="{show: show}">
    <div v-for="(item, index) in items" :key="`menuitem-${index}`" class="menu-item" @click="openWindow(item.window)">
      <div class="menu-item-icon"><img :src="getImgUrl(item.icon)"></div>
      <div class="menu-item-text">{{ item.name }}</div>
    </div>
  </div>
</template>

<script>
import emitter from 'tiny-emitter/instance'

export default {
  data: () => {
    return {
      items: [
        { icon: 'options', name: 'Options', window: { name: 'Options', component: 'options', multipe: false } }
      ],
      show: false
    }
  },
  mounted: function() {
    const self = this;
    emitter.on('show-menu', function(){
      if(self.show) {
        self.show = false
      } else {
        self.show = true
      }
    })
    emitter.on('close-menu', function(){
      self.show = false
    })
  },
  methods: {
    getImgUrl: function(img) {
      return require(`@/assets/icons/${img}.svg`)
    },
    openWindow: function(window) {
      emitter.emit('open-window', window)
    }
  }
}
</script>

<style>
#menu {
  position: absolute;
  z-index: 100000;
  left: 0;
  bottom: 45px;
  width: 200px;
  background: #FFF;
  background: rgba(255,255,255,0.3);
  backdrop-filter: blur(1.5rem);
  opacity: 0;
  visibility: hidden;
}

#menu.show {
  opacity: 1;
  visibility: visible;
}

.menu-item {
  display: flex;
  cursor: pointer;
  padding: 0.6rem 1rem;
  align-items: center;
  background: transparent;
  transition: background 300ms ease-in-out;
}

.menu-item:hover {
  background: #f5f5f5;
}

.menu-item:hover .menu-item-text {
  color: rgb(26, 26, 26);
  text-shadow: 0px 0px 0 rgba(0,0,0,0);
}

.menu-item-icon > img {
  width: 30px;
  height: auto;
  margin-right: 1rem;
}

.menu-item-text {
  font-size: 1.2rem;
  color: #FFF;
  text-shadow: 0px 0px 2px rgba(0,0,0,0.8);
  transition: all 300ms ease-in-out;
}
</style>