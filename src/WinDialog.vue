<template>
<v-dialog v-model="showWinDialog" persistent max-width="300">
  <v-card>
    <v-card-text>
      <p>Parece que eu acertei de novo...</p>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn outline flat="flat" @click.native="$emit('reset-game')">Reiniciar</v-btn>
    </v-card-actions>
  </v-card>
</v-dialog>
</template>

<script>
export default {
  name: 'win-dialog',
  props: ['showWinDialog']
}
</script>
