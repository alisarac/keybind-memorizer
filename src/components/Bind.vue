<template>
  <div>
    <img v-if="spell.icon" :src="icon"/>
    {{ spell.name }} - {{ spell.bind }}
    <button @click="recordSequence()">Record Bind</button>
  </div>
</template>

<script>
// import Mousetrap from 'mousetrap'
/* global console */
var Mousetrap = require('mousetrap-record')(require('mousetrap'));

export default {
  name: 'bind-key',
  props: ['spell'],
  data () {
    return {
    }
  },
  computed: {
    icon() {
      return "https://wow.zamimg.com/images/wow/icons/large/"+this.spell.icon+".jpg"
    }
  },
  mounted(){
    Mousetrap.bind([this.spell.bind], (e) => {
      console.log(this.spell.bind, ' pressed ', this.spell.name)
      return false;
    });
  },
  unmounted(){
    Mousetrap.unbind([this.spell.bind]);
  },
  methods: {
    recordSequence() {
      Mousetrap.record((sequence) => {
        this.bindkey = sequence.join(' ')
        this.setBind()
      });
    }
  }

}
</script>

<style scoped>
img {
    border-radius: 10%;
    width: 50px;
    border: 2px solid grey;
}
</style>

