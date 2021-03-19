<template>
  <div class="app">
    <div class="app-icon"><img :src="getImgUrl(icon)"></div>
    <div class="app-desc">{{ desc }}</div>
  </div>
</template>

<script>
export default {
  props: {
    icon: { default: '', type: String },
    desc: { default: '', type: String }
  },
  methods: {
    getImgUrl: function(img) {
      return require(`@/assets/icons/${img}.svg`)
    }
  }
}
</script>

<style>
.app {
  display: flex;
  flex-direction: column;
  width: 100px;
  margin: 1rem;
  padding: 0.5rem
}

.app:hover {
  background: rgba(0,0,0,0.3);
  background: rgba(255,255,255,0.3);
  backdrop-filter: blur(1.5rem);
}

.app-icon > img {
  width: 40px;
}

.app-desc {
  font-size: 0.8rem;
  text-align: center;
  color: #FFF;
  text-shadow: 0px 0px 2px rgba(0,0,0,0.8);
}

.app-icon {
  text-align: center;
}
</style>