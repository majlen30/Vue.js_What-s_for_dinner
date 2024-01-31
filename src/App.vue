<template>
  <header>
    <h1>What's for dinner? (fast i vite)</h1>
  </header>
  <SearchBox></SearchBox>
  <div id="recipeSuggestions">
    <CategorySuggestion category="Beef"></CategorySuggestion>
    <CategorySuggestion category="Seafood"></CategorySuggestion>
    <CategorySuggestion category="Chicken"></CategorySuggestion>
  </div>
</template>

<script>
import CategorySuggestion from './components/CategorySuggestion.vue'
import SearchBox from './components/SearchBox.vue'

export default {
  components: {
    CategorySuggestion,
    SearchBox,
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
