<template>
  <section>
    <article>
      <div>
        <h2>{{ title }}</h2>
        <img alt="Picture of the recipe" :src="thumbNail" />
      </div>

      <!-- <div v-if="ingredients !== null"> -->
      <ul v-for="i in ingredients">
        <li>{{ i }}</li>
      </ul>
      <!-- </div>
      <div v-else>
        <h2>Loading...</h2>
      </div> -->

      <h3>Instructions:</h3>

      <p>{{ instructions }}</p>
    </article>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

// let meal = ref({ ingredients: null })
let ingredients = ref(null),
  thumbNail = ref(null),
  title = ref(null),
  instructions = ref(null)

createRecipe()
// test()

function createRecipe() {
  axios
    .get(`https://www.themealdb.com/api/json/v1/1/lookup.php?i=52839`)
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

// async function test() {
//   try {
//     const res = await axios.get(
//       `https://www.themealdb.com/api/json/v1/1/lookup.php?i=52839`
//     )
//     let recipe = result.data.meals[0],
//       ingredient = [],
//       measure = []

//     for (let property in recipe) {
//       if (property.startsWith('strIngredient')) {
//         ingredient.push(property)
//       }
//       if (property.startsWith('strMeasure')) {
//         measure.push(property)
//       }
//     }

//     // -----------------------------------------------------------------------
//     // Loop som filtrerar alla aktiva nycklar
//     meal = {
//       ingredients: [],
//     }

//     for (let i = 0; i < ingredient.length; i++) {
//       if (recipe[ingredient[i]] !== null && recipe[ingredient[i]] !== '') {
//         meal.ingredients.push(`${recipe[measure[i]]} ${recipe[ingredient[i]]}`)
//       }
//     }

//     console.log(meal.ingredients)
//   } catch (error) {
//     // Handle errors
//   }
// }
</script>

<style></style>
