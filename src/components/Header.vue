<template>
  <header>
    <div id="nav">
      <router-link v-for="(route, index) in routes" :key="`routelink-${index}`" :to="route.path">
        <a :href="route.path">{{ route.name }}</a><span v-if="index < (routes.length - 1)">&nbsp;|&nbsp;</span>
      </router-link>
    </div>
  </header>
</template>

<script>
import routes from './../router'

export default {
  data() {
    return {
      routes: routes.options.routes
    }
  }
}
</script>

<style>

</style>