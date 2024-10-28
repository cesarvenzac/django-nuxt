<template>
  <main v-if="recipe">
    <div>
      <div>
        <h2>{{ recipe.name }}</h2>
      </div>
      <div>
        <img
          style="width: 200px"
          :src="recipe.picture"
          alt="Recipe Image" />
      </div>
      <div>
        <div>
          <h4>Ingrédients</h4>
          <p>{{ recipe.ingredients }}</p>
          <h4>Temps de préparation ⏱</h4>
          <p>{{ recipe.prep_time }} mins</p>
          <h4>Difficulté</h4>
          <p>{{ recipe.difficulty }}</p>
          <h4>Guide de préparation</h4>
          <textarea
            rows="10"
            v-html="recipe.prep_guide"
            disabled />
        </div>
      </div>
    </div>
  </main>
  <span v-else>Chargement...</span>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

const recipe = ref(null);
const route = useRoute();

onMounted(async () => {
  try {
    const response = await $fetch(
      `http://localhost:8000/api/recipes/${route.params.id}/`
    );
    recipe.value = response;
  } catch (error) {
    console.error("Error fetching recipe:", error);
  }
});
</script>

<style scoped></style>
