<!-- pages/toto.vue => localhost:3000/toto -->

<template>
  <div class="body">
    <h1>SpeechMobile</h1>

    <h1 v-if="isActive">
      <h2 class="bienvenue"></h2>
      <h2>{{item}}</h2>
      <div class="button">
        <button class="choix" @click="submit1()">OUI</button>

        <button class="choix" @click="isActive">NON</button>
      </div>
    </h1>

    <h1 v-else-if="!isActive">
      <h2 class="bienvenue">Bienvenue</h2>
      <button
        class="cercle"
        v-for="item in lignesOK"
        :key="item"
        @click="speak(item),isActive= !isActive"
      >
        <!--@click="speak(item),submit(item),isActive= !isActive"-->
        <h2>{{item}}</h2>
      </button>
    </h1>
  </div>
</template>
<script>
import VueRouter from 'vue-router'
import Vue from 'vue'

Vue.use(VueRouter)

export default {
  data: function() {
    return {
      message: '',
      tab: [],
      lignesOK: [],
      isActive: false
    }
  },

  methods: {
    submit1(item) {
      this.$router.push({
        path: '/direction'
      })
    },
    submit2(item) {
      this.$router.push({
        path: '/toto'
      })
    },

    speak(item) {
      var msg = new SpeechSynthesisUtterance()
      var voices = window.speechSynthesis.getVoices()
      msg.voice = voices[0]
      msg.voiceURI = 'native'
      msg.volume = 1
      msg.rate = 1
      msg.pitch = 0.8
      msg.text = 'Vous avez choisi la ligne ' + item + ' Oui ou Non'
      console.log(item)
      msg.lang = 'fr-CA'
      speechSynthesis.speak(msg)
    }
  },
  mounted: function() {
    this.$axios
      .$get(
        'http://marcelle-mobi-api.herokuapp.com/vehicules/rtm?grant_token=code4marseillefrioul#'
      )
      .then(response => {
        this.tab = response

        let lignes = []
        //let lignesOK = []
        let precedent = ''
        for (let Element in this.tab) {
          lignes.push(this.tab[Element].nomLigneCial)
          console.log(this.tab[Element].nomLigneCial)
        }
        //tri tableau
        lignes.sort()
        for (let i = 0; i < lignes.length; i++) {
          if (lignes[i] != precedent) {
            this.lignesOK.push(lignes[i])
            console.log(this.lignesOK)
          }
          precedent = lignes[i]
        }
      })
  }
}
</script>
<style scoped>
h1 {
  text-align: center;
  font-size: 85px;
}

h2 {
  text-align: center;
  font-size: 85px;
}

h3 {
  color: red;
}

.bienvenue {
  font-size: 50px;
}

.cercle {
  display: block;
  width: 200px;
  height: 200px;
  margin: 25px auto;
  background-color: white;
  border-radius: 50%;
  color: black;
  text-align: center;
}

.body {
  background: #25a9e8;
  color: white;
}

.choix {
  color: white;
  text-decoration: none;
  font-size: 100px;
  font-weight: bold;
  width: 95%;
  background: #66c3ef;
  border: none;
  padding: 10px;
  margin: 30px;
  margin-top: 50px;
}
</style>
