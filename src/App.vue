<template>
  <div id="app">
    <button
      @click="importando = !importando"
    >
      Importar Participantes
    </button>
    <button
      @click="sortear"
      :disabled="importando || participantes.length === 0"
    >
      Sortear
    </button>

    <AppImporter
      v-if="importando"
      @importar="importarParticipantes"
    />

    <pre v-if="vencedor">{{ vencedor }}</pre>
  </div>
</template>

<script>
import AppImporter from './components/AppImporter'

export default {
  name: 'App',
  components: {
    AppImporter
  },
  data: () => ({
    importando: false,
    participantes: [],
    vencedor: '',
    fps: 20,
    now: null,
    then: Date.now(),
    interval: 0,
    delta: null,
    animationFrame: null,
  }),
  methods: {
    importarParticipantes (participantes) {
      this.participantes = participantes
      this.importando = false
    },
    sortear () {
      this.embaralhar(this.participantes)
      this.desenhar()
      this.suavizar()
    },
    embaralhar (array) {
      let currentIndex = array.length, temporaryValue, randomIndex

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {

        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex)
        currentIndex -= 1

        // And swap it with the current element.
        temporaryValue = array[currentIndex]
        array[currentIndex] = array[randomIndex]
        array[randomIndex] = temporaryValue
      }

      return array
    },
    desenhar () {
      this.animationFrame = requestAnimationFrame(this.desenhar)
      this.interval = 1000 / this.fps
      this.now = Date.now()
      this.delta = this.now - this.then
      if (this.delta > this.interval) {
        this.then = this.now - (this.delta % this.interval)
        const random = this.obterNumeroAleatorio(this.participantes.length)
        this.vencedor = this.participantes[random]
      }
    },
    finalizar () {
      cancelAnimationFrame(this.animationFrame)
      this.limpar()
    },
    limpar () {
      Object.assign(this.$data, {
        ...this.$options.data(),
        vencedor: `O vencedor é \n${this.vencedor}`,
        participantes: this.participantes
      })
    },
    suavizar () {
      setTimeout(() => {
        const loop = setInterval(() => {
          if (this.fps === 2) {
            clearInterval(loop)
            this.finalizar()
          }
          this.fps--
        }, 250)
      }, 1500)
    },
    obterNumeroAleatorio (max) {
      let min = 0
      max = Math.floor(max)
      return Math.floor(Math.random() * (max - min)) + min
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 20px;
}

pre {
  color: #ffffff;
  background-color: #027d02;
  padding: 10px;
  border-radius: 2px;
  text-align: center;
  margin-top: 10px;
  font-size: 3rem;
}
</style>
