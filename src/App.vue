<template>
  <div id="app" @click.self="filteredList = []" style="border:1px solid black;  height: 100vh; padding: 20px;">
    <div>
      <h3 v-if="selectedItem">{{ selectedItem.name }}</h3>
      <input v-model="value" placeholder="Search Item"
        @focus="filteredList = ingredients"
        @input="updateList">
      <div v-for="ingridient in filteredList" v-bind:key="ingridient.id">
        <button style="width: 100%; text-align: left;" @click="selectItem(ingridient)">{{ ingridient.name }}</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      selectedItem: null,
      value: '',
      filteredList: [],
      ingredients: null
    }
  },
  async mounted() {
    try {
      this.ingredients = await this.fetchIngridients()
    }
    catch (err) {
      console.error(err)
    }
  },
  methods: {
    selectItem(ingridient) {
      this.selectedItem = ingridient
      this.filteredList = []
      this.value = ''
    },
    updateList() {
      this.filteredList = this.ingredients.filter(ingridient => {
        if (ingridient.name.toLocaleLowerCase().includes(this.value.toLocaleLowerCase())) {
          return ingridient
        }
      })
      this.filteredList.sort((a,b) => {
          if (a.name.toLocaleLowerCase() < b.name.toLocaleLowerCase()) return -1
          if (a.name.toLocaleLowerCase() > b.name.toLocaleLowerCase()) return 1
          return 0
      })
    },
    async fetchIngridients() {
      try {
        const url = "https://backend-challenge-production.up.railway.app/ingredients"
        const response = await axios.get(url)
        return response.data
      }
      catch(err) {
        throw err
      }
    }
  }
}
</script>
