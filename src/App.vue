<template>
<v-content id="app">
  <v-app>
    <v-layout align-center>
      <v-flex xs12 sm4 offset-sm4 align-center>
        <v-card v-if="firstMessage" class="mx-2 elevation-10">
          <v-card-text>
            <p class="mb-0">Pense em um prato...</p>
          </v-card-text>
          <v-card-actions>
            <v-btn block outline @click="firstMessage = false">Ok</v-btn>
          </v-card-actions>
        </v-card>
        <v-card v-else class="mx-2 elevation-10">
          <v-card-text>
            <p class="mb-0" v-if="!isTypeSelected">O prato que você pensou é {{ selectedTypeName }}?</p>
            <p class="mb-0" v-else>O prato que você pensou é {{ selectedFoodName }}?</p>
          </v-card-text>
          <v-card-actions>
            <v-btn block outline @click="chosenNo">Não</v-btn>
            <v-btn block outline @click="chosenYes">Sim</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
      <win-dialog :show-win-dialog="showWinDialog" @reset-game="resetGame"></win-dialog>
      <add-dialog :show-add-dialog="showAddDialog" @add-food="addFood"></add-dialog>
    </v-layout>
  </v-app>
</v-content>
</template>

<script>
import _ from 'lodash'
import WinDialog from './WinDialog'
import AddDialog from './AddDialog'

export default {
  name: 'app',
  components: {
    'win-dialog': WinDialog,
    'add-dialog': AddDialog
  },
  data: () => ({
    firstMessage: true,
    showWinDialog: false,
    showAddDialog: false,
    selectedTypeIndex: 0,
    selectedFoodIndex: 0,
    isTypeSelected: false,
    options: {
      massa: ['lasanha'],
      bolo: ['bolo de chocolate']
    }
  }),
  computed: {
    selectedTypeName() {
      return _.keys(this.options)[this.selectedTypeIndex]
    },
    selectedFoodName() {
      return this.options[this.selectedTypeName][this.selectedFoodIndex]
    }
  },
  methods: {
    chosenYes() {
      if (this.isTypeSelected) this.finishGame()
      else this.isTypeSelected = true
    },
    chosenNo() {
      if (this.isTypeSelected) {
        if (this.existNextFood()) {
          this.selectedFoodIndex++
        } else {
          this.showAddDialog = true
        }
      } else {
        if (this.existNextType()) {
          this.selectedTypeIndex++
        } else {
          this.showAddDialog = true
        }
      }
    },
    existNextType() {
      return Boolean(_.keys(this.options)[this.selectedTypeIndex + 1])
    },
    existNextFood() {
      return Boolean(this.options[this.selectedTypeName][this.selectedFoodIndex + 1])
    },
    addFood(food) {
      if (this.options[food.type]) this.options[food.type].push(food.name)
      else this.$set(this.options, food.type, [food.name])
      this.resetGame()
    },
    finishGame() {
      this.showWinDialog = true
    },
    resetGame() {
      this.isTypeSelected = false
      this.selectedTypeIndex = 0
      this.selectedFoodIndex = 0
      this.showWinDialog = false
      this.showAddDialog = false
      this.firstMessage = true
    }
  }
}
</script>

<style>
#app {
  font-family: 'PT Sans', sans-serif;
  background: -webkit-radial-gradient(rgb(255, 90, 17), rgb(255, 108, 43));
  background: radial-gradient(rgb(255, 90, 17), rgb(255, 108, 43));
}

p {
  font-size: 16px;
}
</style>
