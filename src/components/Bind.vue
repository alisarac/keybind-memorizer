<template>
  <div>
    <input type="text" v-model="bindname"/>
    <input type="text" v-model="iconname"/>
    <img v-if="iconname" :src="icon"/>
    {{ bindname }} - {{ bindkey }}
    <button @click="recordSequence()">Record Bind</button>
  </div>
</template>

<script>
// import Mousetrap from 'mousetrap'
/* global console */
var Mousetrap = require('mousetrap-record')(require('mousetrap'));

export default {
  name: 'bind-key',
  props: {
  },
  data () {
    return {
      bindkey: null,
      bindname: null,
      iconname: null,
      editable: true,
      previousKey: null
    }
  },
  computed: {
    icon() {
      return "https://wow.zamimg.com/images/wow/icons/large/"+this.iconname+".jpg"
    }
  },
  methods: {
    setBind () {
      console.log('setting bind', this.bindkey, this.bindname)
      if(this.previousKey) {
        Mousetrap.unbind([this.previousKey])
      }
      Mousetrap.bind([this.bindkey], () => {
        this.previousKey = this.bindkey
        console.log(this.bindkey, ' pressed ', this.bindname)
        return false;
      });
    },
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

