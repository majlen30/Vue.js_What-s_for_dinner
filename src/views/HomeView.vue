<template>
  <Title></Title>
  <SearchBox></SearchBox>
  <div id="recipeSuggestions">
    <CategorySuggestion category="Pasta"></CategorySuggestion>
    <CategorySuggestion category="Seafood"></CategorySuggestion>
    <CategorySuggestion category="Dessert"></CategorySuggestion>
  </div>
</template>

<script>
import Title from '../components/Title.vue'
import CategorySuggestion from '../components/CategorySuggestion.vue'
import SearchBox from '../components/SearchBox.vue'

export default {
  components: {
    Title,
    CategorySuggestion,
    SearchBox,
  },
}
</script>

<style>
#recipeSuggestions {
  display: flex;
}
</style>
