<template>
<v-dialog v-model="showAddDialog" persistent max-width="300">
  <v-card v-if="firstStep">
    <v-card-text>
      <p class="mb-0">Qual prato você pensou?</p>
      <v-text-field label="Nome do prato" class="input-group--focused" @keyup.enter="next" autofocus hide-details single-line v-model="food.name"></v-text-field>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn outline @click="next" :disabled="!food.name">Continuar</v-btn>
    </v-card-actions>
  </v-card>
  <v-card v-else>
    <v-card-text>
      <p class="mb-0">O que é {{ food.name }}?</p>
      <v-text-field label="Tipo do prato" class="input-group--focused" @keyup.enter="finish" autofocus hide-details single-line v-model="food.type"></v-text-field>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn outline @click="finish" :disabled="!food.type">Reiniciar</v-btn>
    </v-card-actions>
  </v-card>
</v-dialog>
</template>

<script>
export default {
  name: 'add-dialog',
  props: ['showAddDialog'],
  data: () => ({
    food: {
      name: '',
      type: ''
    },
    firstStep: true
  }),
  methods: {
    next() {
      this.firstStep = false
    },
    finish() {
      const food = {
        name: String(this.food.name).toLowerCase(),
        type: String(this.food.type).toLowerCase()
      }
      this.food.name = ''
      this.food.type = ''
      this.$emit('add-food', food)
      this.firstStep = true
    }
  }
}
</script>
