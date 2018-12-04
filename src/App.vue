<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <button @click="addBinds"> Add New Bind</button>
    <ul>
      <li v-for="bind in binds">
        <bind :key="bind"/>
      </li>
    </ul>

  </div>
</template>

<script>
import Bind from './components/Bind.vue'

export default {
  name: 'app',
  components: {
    Bind
  },
  data () {
    return {
      binds: [],
      startKey: 1
    }
  },
  methods: {
    addBinds(){
      this.binds.push(this.startKey)
      this.startKey++
    }
  },
  mounted () {

  },
  unmounted () {
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
