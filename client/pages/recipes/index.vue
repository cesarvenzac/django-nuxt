<template>
  <main>
    <div>
      <template
        v-for="recipe in recipes"
        :key="recipe.id">
        <div>
          <recipe-card
            :onDelete="deleteRecipe"
            :recipe="recipe"></recipe-card>
        </div>
      </template>
    </div>
  </main>
</template>

<script setup>
import RecipeCard from "~/components/RecipeCard.vue";
import { useFetch } from "#app";

definePageMeta({
  title: "Recipes list",
});

const { data: recipes, refresh } = await useFetch(
  "http://localhost:8000/api/recipes"
);

async function deleteRecipe(recipeId) {
  try {
    await $fetch(`http://localhost:8000/api/recipes/${recipeId}/`, {
      method: "DELETE",
    });
    await refresh();
  } catch (e) {
    console.error("Failed to delete recipe:", e);
  }
}
</script>

<style scoped></style>
