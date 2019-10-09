<!-- pages/direction.vue => localhost:3000/direction -->

<template>
  <div class="body">
    <h1>SpeechMobile</h1>

    <h1>Votre bus</h1>
    <h3 class="prochain">Prochain passage</h3>

    <div class="passage">
      <h1>4mn</h1>
    </div>

    <div>
    <h1>14mn</h1>
    </div>
    <div>
        <button @click="speak()">Suivant</button>
    </div>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      message: '',
      isActive: false
    }
  },

  methods: {
    speak(item) {
      var msg = new SpeechSynthesisUtterance()
      var voices = window.speechSynthesis.getVoices()
      msg.voice = voices[0]
      msg.voiceURI = 'native'
      msg.volume = 1
      msg.rate = 1
      msg.pitch = 0.8
      msg.text =' le passage du prochain bus est dans 14 minutes'
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
  font-size: 50px;
  margin-left: 600px;
  border: none !important;
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

.prochain {
  color: white;
  text-align: center;
}

p {
  text-align: center;
}
.suivant {
  text-align: center;
}
</style>
