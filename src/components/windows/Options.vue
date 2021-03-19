<template>
  <div>Background</div>
  <div class="selecter">
    <img v-for="(item, index) in backgrounds" :key="`background-${index}`" :src="getImgUrl(item)" @click="setBackground(item)">
  </div>
</template>

<script>
import emitter from 'tiny-emitter/instance'

export default {
  data: () => {
    return {
      backgrounds: [1,2]
    }
  },
  methods: {
    getImgUrl: function(img) {
      return require(`@/assets/bg-${img}.jpg`)
    },
    setBackground: function(bg) {
      emitter.emit('set-background', bg)
    }
  }
}
</script>

<style>
.selecter {
  margin: 0 -1rem;
}
.selecter img {
  width: 80px;
  height: auto;
  margin: 1rem;
  cursor: pointer;
  opacity: 1;
  transition: opacity 300ms ease-in-out;
}

.selecter img:hover {
  opacity: 0.8;
}
</style>