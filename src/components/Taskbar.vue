<template>
  <div id="taskbar">
    <div @click="showMenu" class="taskbar-menu"><img class="taskbar-menu-icon" src="@/assets/icons/menu.svg"></div>
    <div class="taskbar-elements"></div>
    <div class="taskbar-info">
      <div class="taskbar-time">{{ time }}</div>
      <div class="taskbar-date">{{ date }}</div>
    </div>
  </div>
</template>

<script>
import emitter from 'tiny-emitter/instance'

export default {
  data: () => {
    return {
      date: '',
      time: ''
    }
  },
  mounted: function() {
    const self = this;
    self.setDateTime();
    setInterval(self.setDateTime, 1000);
  }, 
  methods: {
    showMenu: function() {
      emitter.emit('show-menu')
    },
    setDateTime: function() {
      const today = new Date()
      const dd = String(today.getDate()).padStart(2, '0');
      const mm = String(today.getMonth() + 1).padStart(2, '0'); //January is 0!
      const yyyy = today.getFullYear();
      this.date = `${dd}.${mm}.${yyyy}`

      let min = today.getMinutes()
      let hour = today.getHours()

      if(min < 10) min = `0${min}`
      if(hour < 10) hour = `0${hour}`

      this.time = `${hour}:${min}`
    }
  }
}
</script>

<style>
#taskbar {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 0.4rem 1rem;
  background: var(--grey);
  background: var(--glass);
  backdrop-filter: blur(1.5rem);
  display: flex;
  flex-direction: row;
  font-size: 0.7rem;
  height: var(--taskbarheight);
}

.taskbar-info {
  margin-left: auto;
  text-align: center;
  line-height: 1rem;
  color: #FFF;
  text-shadow: 0px 0px 2px rgba(0,0,0,0.8);
}

.taskbar-menu {
  display: flex;
  align-items: center;
  justify-content: center;
}

.taskbar-menu-icon {
  width: 30px;
  height: auto;
  cursor: pointer;
}
</style>