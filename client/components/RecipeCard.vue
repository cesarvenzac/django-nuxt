<template>
  <div class="recipe">
    <img
      style="width: 200px"
      :src="recipe.picture" />
    <div class="infos">
      <h5>{{ recipe.name }}</h5>
      <p><strong>Ingrédients :</strong> {{ recipe.ingredients }}</p>
      <div class="options">
        <nuxt-link :to="`/recipes/${recipe.id}/`">Voir</nuxt-link>
        <nuxt-link :to="`/recipes/${recipe.id}/edit/`">Modifier</nuxt-link>
        <button @click="onDelete(recipe.id)">Supprimer</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["recipe", "onDelete"],
};
</script>

<style>
.recipe {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 8px;
  border: 1px solid black;
  border-radius: 8px;
}
.infos {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.options {
  display: flex;
  gap: 8px;
}
</style>
