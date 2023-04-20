<template>
  <div id="app">
    <div> 
      <input v-model="value" placeholder="Search Item" @click="showList" @input="updateList">
      <div v-for="ingridient in filteredList" v-bind:key="ingridient.id">
        <p>{{ ingridient.name }}</p>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
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
    showList() {
      this.filteredList = this.ingredients
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
