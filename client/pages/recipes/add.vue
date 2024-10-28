<template>
  <main>
    <div>
      <div>
        <h2>{{ recipe.name }}</h2>
      </div>
      <div>
        <img
          v-if="preview"
          style="width: 200px"
          :src="preview"
          alt />
        <img
          v-else
          style="width: 200px"
          src="/images/placeholder.jpg" />
      </div>
      <div>
        <form @submit.prevent="submitRecipe">
          <div>
            <label for>Nom de la recette</label>
            <input
              type="text"
              v-model="recipe.name" />
          </div>
          <div>
            <label for>Ingredients</label>
            <input
              v-model="recipe.ingredients"
              type="text" />
          </div>
          <div>
            <label for>Photo du plat</label>
            <input
              type="file"
              name="file"
              @change="onFileChange" />
          </div>
          <div>
            <div>
              <div>
                <label for>Difficulté</label>
                <select v-model="recipe.difficulty">
                  <option value="Easy">Facile</option>
                  <option value="Medium">Moyen</option>
                  <option value="Hard">Difficile</option>
                </select>
              </div>
            </div>
            <div>
              <div>
                <label for>
                  Temps de préparation
                  <small>(minutes)</small>
                </label>
                <input
                  v-model="recipe.prep_time"
                  type="number" />
              </div>
            </div>
          </div>
          <div>
            <label for>Guide de préparation</label>
            <textarea
              v-model="recipe.prep_guide"
              rows="8"></textarea>
          </div>
          <button type="submit">Ajouter</button>
        </form>
      </div>
    </div>
  </main>
</template>

<script setup>
import { useRouter } from "vue-router";
import { ref } from "vue";

const recipe = ref({
  name: "",
  picture: "",
  ingredients: "",
  difficulty: "",
  prep_time: null,
  prep_guide: "",
});

const preview = ref("");
const router = useRouter();

const onFileChange = (e) => {
  const files = e.target.files || e.dataTransfer.files;
  if (!files.length) return;
  recipe.value.picture = files[0];
  createImage(files[0]);
};

const createImage = (file) => {
  const reader = new FileReader();
  reader.onload = (e) => {
    preview.value = e.target.result;
  };
  reader.readAsDataURL(file);
};

const submitRecipe = async () => {
  const formData = new FormData();
  for (const key in recipe.value) {
    formData.append(key, recipe.value[key]);
  }

  try {
    await $fetch("http://localhost:8000/api/recipes/", {
      method: "POST",
      body: formData,
    });
    router.push("/recipes/");
  } catch (error) {
    console.error("Error submitting recipe:", error);
  }
};
</script>

<style scoped></style>
