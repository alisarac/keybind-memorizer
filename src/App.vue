<template>
  <div id="app">
    <div class="container">
      <span v-for="spell in spells" :key="spell.bind">
        <bind :spell="spell" :myturn="currentBind == spell" @done="done" @failed="failed" ref="bindComponent"/>
      </span>
    </div>
    <div class="randomized" v-if="currentBind">
      <img v-if="currentBind.icon" :src="`https://wow.zamimg.com/images/wow/icons/large/${currentBind.icon}.jpg`"/>
      <div>{{ currentBind.name }}
        <div class="w3-light-grey">
          <div class="w3-green" :style="`height:24px;width:${100-(timer/20)}%`"></div>
        </div>
      </div>
    </div>
    <div class="randomized" v-if="lastResult">
      <div> {{ lastResult }} </div>
    </div>
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
      spells: [
        {bind: '1', name: 'Power Word: Shield', icon: 'spell_holy_powerwordshield'},
        {bind: '2', name: 'Penance', icon: 'spell_holy_penance'},
        {bind: '3', name: 'Shadow Mend', icon: 'spell_shadow_shadowmend'},
        {bind: '4', name: 'Power Word: Radiance', icon: 'spell_priest_power-word'},
        {bind: 'ctrl+1', name: 'Rapture', icon: 'spell_holy_rapture'},
        {bind: 'ctrl+2', name: 'Pain Supression', icon: 'spell_holy_painsupression'},
        {bind: 'ctrl+3', name: 'Power Word: Barrier', icon: 'spell_holy_powerwordbarrier'},
        {bind: 'ctrl+4', name: 'Desperate Prayer', icon: 'spell_holy_testoffaith'},
        {bind: 'r', name: 'Premonition', icon: 'inv_helm_robe_raidpriest_k_01'},
        {bind: 'f', name: 'Mind Control', icon: 'spell_shadow_shadowworddominate'},
        {bind: '5', name: 'Purge Of the Wicked', icon: 'ability_mage_firestarter'},
        {bind: '6', name: 'Smite', icon: 'spell_holy_holysmite'},
        {bind: '7', name: 'Mindbender', icon: 'spell_shadow_soulleech_3'},
        {bind: '8', name: 'Dread Gladiator\'s Badge', icon: 'spell_holy_championsbond'},
        {bind: '9', name: 'Power Word: Solace', icon: 'ability_priest_flashoflight'},
        {bind: '0', name: 'Angelic Feather', icon: 'ability_priest_angelicfeather'},
        {bind: 'f5', name: 'Mass Dispel', icon: 'spell_arcane_massdispel'},
        {bind: 'f6', name: 'Psychic Scream', icon: 'spell_shadow_psychicscream'},
        {bind: 'f7', name: 'Dark Archangel', icon: 'ability_priest_darkarchangel'},
        {bind: 'f8', name: 'Gladiator\'s Medallion', icon: 'ability_pvp_gladiatormedallion'},
        {bind: 'f9', name: 'Dispel Magic', icon: 'spell_nature_nullifydisease'},
        {bind: 'f10', name: 'Will of the Forsaken', icon: 'spell_shadow_raisedead'},
        {bind: 'ctrl+5', name: 'Archangel', icon: 'ability_priest_archangel'},
        {bind: 'ctrl+6', name: 'Leap of Faith', icon: 'priest_spell_leapoffaith_a'},
        {bind: 'ctrl+7', name: 'Power Word: Fortitude', icon: 'spell_holy_wordfortitude'},
        {bind: 'ctrl+8', name: 'Yemek', icon: 'inv_misc_food_73cinnamonroll'},
        {bind: 'ctrl+9', name: 'Holy Nova', icon: 'spell_holy_holynova'},
        {bind: 'ctrl+0', name: 'Purify', icon: 'spell_holy_dispelmagic'},
        {bind: 'ctrl+f5', name: 'Levitate', icon: 'spell_holy_layonhands'},
        {bind: 'ctrl+f6', name: 'Resurrection', icon: 'spell_holy_resurrection'},
        {bind: 'ctrl+f7', name: 'Mass Resurrection', icon: 'achievement_guildperk_massresurrection'},
        {bind: 'ctrl+f8', name: 'Mount', icon: 'ability_mount_kodo_01'},
        {bind: 'ctrl+f9', name: 'Fade', icon: 'spell_magic_lesserinvisibilty'},
        {bind: 'ctrl+f10', name: 'Flying Mount', icon: 'ability_mount_netherdrakepurple'},
      ],
      startKey: 1,
      currentBind: null,
      lastResult: null,
      waiting: true,
      waitingTimeout: null,
      timer: 2000,
      exercises: []
    }
  },
  methods: {
    updateSelectedBind() {
      this.waitingTimeout = setTimeout(this.timerTick, 100)
      this.currentBind = this.spells[this.exercises[Math.floor(Math.random() * this.exercises.length)]];
      this.waiting = true
    },
    timerTick() {
      this.timer -= 50
      if(this.timer <= 0) {
        this.$emit('timeout', this.currentBind);
        this.failed()
        return
      }
      this.waitingTimeout = setTimeout(this.timerTick, 50)
    },
    done() {
      this.waiting = false
      this.timer = 2000
      clearTimeout(this.waitingTimeout);
      this.lastResult = "Correct"
      this.updateSelectedBind()
    },
    failed() {
      this.waiting = false
      this.timer = 2000
      clearTimeout(this.waitingTimeout);
      this.lastResult = "Wrong"
      this.updateSelectedBind()
    }
  },
  mounted () {
    this.updateSelectedBind()
  },
  unmounted () {
  }
}
</script>

<style>
.w3-green, .w3-hover-green:hover {
    color: #fff!important;
    background-color: #4CAF50!important;
    animation: move 0.1s linear;
}
.w3-red, .w3-hover-red:hover {
    color: #fff!important;
    background-color: red!important;
}
.w3-light-grey, .w3-hover-light-grey:hover, .w3-light-gray, .w3-hover-light-gray:hover {
    color: #000!important;
    background-color: #f1f1f1!important;
    width: 200px;
    margin:auto;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  display: flex; /* or inline-flex */
  flex-flow: row wrap;
  justify-content: space-around;
}
.container >span {
  margin-bottom: 20px;
}
.randomized {
  margin-top: 20px;
}
</style>
