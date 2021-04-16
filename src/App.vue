<template>
  <div id="desktop" :style="`background-image: url(${getImgUrl(`bg-${background}.jpg`)})`">
    <login></login>
    <div @click="closeMenu" class="desktop-wrapper">
      <myapp @dblclick="startApp(app.window)" v-for="(app, index) in apps" :key="`desktop-app-${index}`" :icon="app.icon" :desc="app.desc"></myapp>
    </div>
    <window :hash="w.hash" v-for="(w, index) in windows" :key="`window-${index}`" :name="w.name" :component="w.component"></window>
    <mymenu></mymenu>
    <taskbar></taskbar>
  </div>
</template>

<script>
import myapp from './components/App.vue'
import window from './components/Window.vue'
import taskbar from './components/Taskbar.vue'
import mymenu from './components/Menu.vue'
import login from './components/Login.vue'
import emitter from 'tiny-emitter/instance'
import md5 from 'js-md5'

export default {
  name: 'App',
  components: {
    myapp,
    window,
    taskbar,
    mymenu,
    login
  },
  data: () => {
    return {
      apps: [
        { window: false, icon: 'trash', desc: 'Trash' },
        { window: false, icon: 'folder', desc: 'Private Stuff' },
        { window: { name: 'Terminal', component: 'console', multiple: true }, icon: 'terminal', desc: 'Terminal'},
      ],
      windows: [],
      background: 1
    }
  },
  mounted: function() {
    const self = this
    emitter.on('close-window', function(hash){
      self.windows = self.windows.filter((window) => window.hash != hash)
    })
    emitter.on('create-folder', function(foldername){
      self.apps.push({
        window: false,
        icon: 'folder',
        desc: foldername
      })
    })
    emitter.on('open-window', function(window){
      self.startApp(window)
    })
    emitter.on('set-background', function(bg){
      self.background = bg
    })
  },
  methods: {
    closeMenu: function() {
      emitter.emit('close-menu')
    },
    getImgUrl: function(img) {
      return require(`@/assets/${img}`)
    },
    startApp: function(window) {
      if(window) {
        if(!window.multiple) {
          const hasWindow = this.windows.map((window) => window.component === 'options')
          if(hasWindow[0]) return
        }

        const random = Math.round((Math.random() * 100))
        const date = Date.now()
        const hash = md5(window.name + window.component + random + date)
        this.windows.push({
          name: window.name,
          component: window.component,
          hash : hash
        })
      }
    }
  }
}
</script>

<style>
:root {
  --grey: #f5f5f5;
  --glass: rgba(255,255,255,0.3);
  --taskbarheight: 45px;
  --console-scrollbar: #fc9003;
  --console-background: #33485E;
}

*, *::before, *::after {
  box-sizing: border-box;
}

body {
  padding: 0;
  margin: 0;
  font-family: Roboto, sans-serif;
  overflow: hidden;
}

#desktop {
  width: 100vw;
  height: 100vh;
  background-size: cover;
  background-position: center;
}

.desktop-wrapper {
  height: calc(100% - var(--taskbarheight));
  width: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  flex-wrap: wrap;
  align-content: flex-start;
}
</style>
