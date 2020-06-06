<template>
  <div>
    <textarea
      rows="10"
      v-model="linhas"
    />
    <button @click="importarParticipantes">Importar</button>
  </div>
</template>

<script>
export default {
  name: 'AppImporter',
  data: () => ({
    linhas: 'William\n' +
      'Jeudi\n' +
      'Turma do Grupo'
  }),
  methods: {
    importarParticipantes () {
      const participantes = this.linhas
        .split('\n')
        .filter((participante) => participante.length > 0)
      // eslint-disable-next-line no-debugger
      this.$emit('importar', participantes)
    }
  }
}
</script>

<style scoped>
textarea {
  display: block;
  width: 100%;
  resize: vertical;
}
</style>
