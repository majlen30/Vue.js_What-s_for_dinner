<template>
  <section>
    <article>
      <div>
        <h2>{{ title }}</h2>
        <img alt="Picture of the recipe" :src="thumbNail" />
      </div>

      <ul v-for="i in ingredients">
        <li>{{ i }}</li>
      </ul>

      <h3>Instructions:</h3>

      <p>{{ instructions }}</p>
    </article>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'

let ingredients = ref(null),
  thumbNail = ref(null),
  title = ref(null),
  instructions = ref(null),
  route = useRoute()

createRecipe(route.params.id)

function createRecipe(id) {
  axios
    .get(`https://www.themealdb.com/api/json/v1/1/lookup.php?i=${id}`)
    .then((result) => {
      let recipe = result.data.meals[0],
        ingredient = [],
        measure = []

      thumbNail.value = recipe.strMealThumb
      title.value = recipe.strMeal
      instructions.value = recipe.strInstructions

      for (let property in recipe) {
        if (property.startsWith('strIngredient')) {
          ingredient.push(property)
        }
        if (property.startsWith('strMeasure')) {
          measure.push(property)
        }
      }

      // -----------------------------------------------------------------------
      // Loop som filtrerar alla aktiva nycklar
      ingredients.value = []

      for (let i = 0; i < ingredient.length; i++) {
        if (recipe[ingredient[i]] !== null && recipe[ingredient[i]] !== '') {
          ingredients.value.push(
            `${recipe[measure[i]]} ${recipe[ingredient[i]]}`
          )
        }
      }
    })
}
</script>

<style scoped>
section {
  background-color: lightskyblue;
  display: flex;
  justify-content: center;
  margin: 1em;
}

article {
  border: solid white 2px;
  flex-direction: column;
  justify-content: center;
  padding: 3em;
  margin: 1em;
  width: 60vw;
}

div {
  align-items: center;
  display: flex;
}

img {
  margin-bottom: 1em;
  width: 20vw;
}

h2 {
  margin: 1em auto;
}

h3 {
  margin: 1.5em 0 0;
}

li {
  border: none;
  padding: 0;
}
</style>
