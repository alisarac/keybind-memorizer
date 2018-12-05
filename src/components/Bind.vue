<template>
  <div class="image">
    <span class="bind-key">{{ spell.bind }}</span>
    <img v-if="spell.icon" :src="icon"/>
    <div class="right-answer">{{ rightCount }}</div>
    <div class="wrong-answer">{{ wrongCount }}</div>
  </div>
</template>

<script>
// import Mousetrap from 'mousetrap'
/* global console */
var Mousetrap = require('mousetrap-record')(require('mousetrap'));

export default {
  name: 'bind-key',
  props: ['spell', 'myturn'],
  data() {
    return {
      rightCount: 0,
      wrongCount: 0
    }
  },
  created: function() {
    this.$parent.$on('timeout', this.onTimeout);
  },
  computed: {
    icon(){
      return "https://wow.zamimg.com/images/wow/icons/large/"+this.spell.icon+".jpg"
    }
  },
  mounted() {
    Mousetrap.bind([this.spell.bind], () => {
      if(this.myturn) {
        this.rightCount++
        this.$emit('done', {})
      } else {
        this.wrongCount++
        this.$emit('failed', {})
      }
      return false;
    });
  },
  unmounted() {
    Mousetrap.unbind([this.spell.bind]);
  },
  methods: {
    onTimeout(currentBind) {
      if(currentBind.bind == this.spell.bind) {
        this.wrongCount++
        this.$emit('failed', {})
      }
    },
    recordSequence() {
      Mousetrap.record((sequence) => {
        this.bindkey = sequence.join(' ')
        this.setBind()
      });
    },
    mouseWheelHandler(e) {
      var eNew = window.event || e // old IE support
      var delta = Math.max(-1, Math.min(1, (eNew.wheelDelta || -eNew.detail)))
      // console.log(delta > 0)
      return delta > 0
    }
  },

}
</script>

<style scoped>
img {
    border-radius: 10%;
    width: 50px;
    height: 50px;
    border: 1px solid grey;
    margin: auto;
}
span {
  font-size: 12px;
}
.image {
   position: relative;
   width: 52px; /* for IE 6 */
}

.right-answer {
   position: absolute;
   top: 50px;
   left: 0px;
   width: 50%;
   color: white;
   background-color: green;
}
.wrong-answer {
   position: absolute;
   top: 50px;
   left: 26px;
   width: 50%;
   color: white;
   background-color: red;
}
.bind-key {
   position: absolute;
   top: -10px;
   left: 0px;
   width: 100%;
   color: white;
   background-color: black;
}
</style>

