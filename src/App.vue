<template>
  <header>
    <h1>What's for dinner? (fast i vite)</h1>
  </header>
  <SearchBox></SearchBox>
  <Recipe></Recipe>
  <div id="recipeSuggestions">
    <CategorySuggestion category="Pasta"></CategorySuggestion>
    <CategorySuggestion category="Seafood"></CategorySuggestion>
    <CategorySuggestion category="Dessert"></CategorySuggestion>
  </div>
</template>

<script>
import CategorySuggestion from './components/CategorySuggestion.vue'
import SearchBox from './components/SearchBox.vue'
import Recipe from './components/Recipe.vue'

export default {
  components: {
    CategorySuggestion,
    SearchBox,
    Recipe,
  },
}
</script>

<style>
header {
  background-color: lightskyblue;
  padding: 0.5em 2em;
  margin: 1em;
}

#recipeSuggestions {
  display: flex;
}
</style>
