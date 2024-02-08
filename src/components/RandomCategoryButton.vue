<template>
  <input @click="onClick" type="button" value="Generate random category" />
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      allCategories: null,
      generatedCategory: null,
    }
  },
  methods: {
    onClick() {
      this.onRandomCategory()
    },
    onRandomCategory() {
      axios
        .get('https://www.themealdb.com/api/json/v1/1/list.php?c=list')
        .then((result) => {
          this.allCategories = []
          for (let i = 0; i < result.data.meals.length; i++) {
            this.allCategories.push(result.data.meals[i].strCategory)
          }

          let randomNumber = Math.floor(
            Math.random() * result.data.meals.length
          )
          this.generatedCategory = this.allCategories[randomNumber]
          this.$emit('random-category', this.generatedCategory)
        })
    },
  },
}
</script>
