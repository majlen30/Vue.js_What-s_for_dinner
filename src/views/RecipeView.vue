<template>
  <Title></Title>
  <Recipe />
</template>

<script>
import Title from '../components/Title.vue'
import CategorySuggestion from '../components/CategorySuggestion.vue'
import Recipe from '../components/Recipe.vue'
import { RouterView } from 'vue-router'

export default {
  components: {
    Title,
    CategorySuggestion,
    Recipe,
    RouterView,
  },
}
</script>
