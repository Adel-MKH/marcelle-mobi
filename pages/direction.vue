<!-- pages/direction.vue => localhost:3000/direction -->

<template>
  <div class="body">
    <h1>SpeechMobile</h1>

    <h1>Choisissez votre direction</h1>
    <button class="choix" @click="submit1(),speak('Castelanne')">
      <a>Castellane</a>
    </button>
    <button class="choix" @click="submit1(),speak('Luminy')">
      <a href>Luminy</a>
    </button>
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
      isActive: false
    }
  },

  methods: {
    submit1(item) {
      this.$router.push({
        path: '/list_arret'
      })
    },

    speak(item) {
      var msg = new SpeechSynthesisUtterance()
      var voices = window.speechSynthesis.getVoices()
      msg.voice = voices[0]
      msg.voiceURI = 'native'
      msg.volume = 1
      msg.rate = 1
      msg.pitch = 1
      msg.text =
        'Vous avez choisi la direction ' +
        item +
        ' Veuillez choisir votre arret'
      console.log(item)
      msg.lang = 'fr-CA'
      speechSynthesis.speak(msg)
    }
  },
  mounted: function() {
    this.$axios
      .$get(
        'https://marcelle-mobi-api.herokuapp.com/vehicules/rtm?grant_token=code4marseillefrioul#'
      )
      .then(response => {
        this.tab = response
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

button {
  padding: 20px;
  margin: 10px;
  background: #66c3ef;
  border: none;
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
