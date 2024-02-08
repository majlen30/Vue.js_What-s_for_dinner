<template>
  <fieldset>
    <label>Number of keywords: {{ numberOfKeywords }}</label>
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

      <section id="searchResult">
        <ul v-for="meal in meals">
          <li>
            <img alt="Picture of the Meal" :src="meal.thumbNail" />
            <RouterLink :to="`/${meal.id}`">{{ meal.title }}</RouterLink>
          </li>
        </ul>
      </section>
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
  computed: {
    numberOfKeywords() {
      if (this.inputValue.trim() === '') {
        return 0
      } else {
        return this.inputValue.trim().split(' ').length
      }
    },
  },
  methods: {
    fetchSearchResult(searchWord) {
      axios
        .get(
          `https://www.themealdb.com/api/json/v1/1/search.php?s=${searchWord}`
        )
        .then((result) => {
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
  watch: {
    searchText(text) {
      alert(`Search results for "${text}" will be served...`)
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

label {
  padding: 0 1em;
}

p {
  text-align: center;
  margin-bottom: 1.5em;
}

#searchResult {
  background-color: lightskyblue;
  margin: 1em;
}
</style>
