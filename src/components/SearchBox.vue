<template>
  <fieldset>
    <input type="text" v-model="inputValue" />
    <input
      @click="onClick"
      :disabled="inputValue === ''"
      type="button"
      value="Search"
    />
  </fieldset>
  <div v-show="search">
    <div v-if="error">
      <h2>Search result for {{ searchText }} (0 results)</h2>
      <p>
        Unfortunately there is no result for your search. Try something else.
      </p>
    </div>
    <div v-else>
      <h2>
        Search result for {{ searchText }} ({{ numberOfSearchResult }} results)
      </h2>
      <ul v-for="meal in meals">
        <li>
          <img alt="Picture of the Meal" :src="meal.thumbNail" />
          <a :href="meal.id">{{ meal.title }}</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      error: null,
      inputValue: '',
      meals: null,
      numberOfSearchResult: null,
      search: false,
      searchText: null,
    }
  },
  methods: {
    fetchSearchResult(ingredient) {
      axios
        .get(
          `https://www.themealdb.com/api/json/v1/1/filter.php?i=${ingredient}`
        )
        .then((result) => {
          console.log(result.data)
          if (result.data.meals === null) {
            this.error = true
          } else {
            this.error = false
            this.numberOfSearchResult = result.data.meals.length
            this.meals = []
            for (let i = 0; i < result.data.meals.length; i++) {
              let x = {
                title: result.data.meals[i].strMeal,
                thumbNail: result.data.meals[i].strMealThumb,
                id: result.data.meals[i].idMeal,
              }
              this.meals.push(x)
            }
          }
        })
    },
    onClick() {
      this.fetchSearchResult(this.inputValue)
      this.search = true
      this.searchText = this.inputValue
    },
  },
}
</script>

<style>
fieldset {
  background-color: lightskyblue;
  border: none;
  display: flex;
  justify-content: center;
  margin: 1em;
  padding: 2em;
}

p {
  text-align: center;
  margin-bottom: 1.5em;
}
</style>
