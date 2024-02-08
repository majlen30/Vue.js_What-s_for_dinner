<template>
  <section class="categorySuggestion">
    <RandomCategoryButton @random-category="fetchMeals" />
    <h2>{{ categoryTitle }}</h2>
    <ul v-for="meal in meals">
      <li>
        <img alt="Picture of the Meal" :src="meal.thumbNail" />
        <RouterLink :to="`/${meal.id}`">{{ meal.title }}</RouterLink>
      </li>
    </ul>
  </section>
</template>

<script>
import axios from 'axios'
import { RouterLink } from 'vue-router'
import RandomCategoryButton from './RandomCategoryButton.vue'

export default {
  created() {
    this.fetchMeals(this.category)
  },
  data() {
    return {
      categoryTitle: null,
      meals: null,
    }
  },
  methods: {
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
  components: { RouterLink, RandomCategoryButton },
}
</script>

<style>
h2 {
  text-align: center;
}

.categorySuggestion {
  background-color: lightskyblue;
  display: flex;
  flex-direction: column;
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
  padding: 1em;
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
