<template>
  <section class="categorySuggestion">
    <h2>{{ category }}</h2>
    <ul v-for="meal in meals">
      <li>
        <img alt="Picture of the Meal" :src="meal.thumbNail" />
        <a :href="meal.id">{{ meal.title }}</a>
      </li>
    </ul>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  created() {
    this.fetchCategories()
    this.fetchMeals(this.c)
  },
  data() {
    return {
      categories: null,
      c: this.category,
      meals: null,
    }
  },
  methods: {
    fetchCategories() {
      axios
        .get('https://www.themealdb.com/api/json/v1/1/list.php?c=list')
        .then((result) => {
          this.categories = []
          for (let i = 0; i < result.data.meals.length; i++) {
            this.categories.push(result.data.meals[i].strCategory)
          }
        })
    },
    fetchMeals(category) {
      axios
        .get(`https://www.themealdb.com/api/json/v1/1/filter.php?c=${category}`)
        .then((result) => {
          this.meals = []
          this.categoryTitle = category
          for (let i = 0; i < result.data.meals.length; i++) {
            let x = {
              title: result.data.meals[i].strMeal,
              thumbNail: result.data.meals[i].strMealThumb,
              id: result.data.meals[i].idMeal,
            }
            this.meals.push(x)
          }
        })
    },
  },
  props: {
    category: String,
  },
}
</script>

<style>
h2 {
  text-align: center;
}

.categorySuggestion {
  background-color: lightskyblue;
  margin: 1em;
  margin-top: 0;
  padding: 1em;
  width: 30%;
}

img {
  padding: 0 1em 0;
  width: 100px;
}

ul {
  margin: 0;
  padding: 0;
}

li {
  align-items: center;
  display: flex;
  list-style: none;
  margin-bottom: 1em;
  padding: 0;
}

a {
  align-items: center;
}
</style>
