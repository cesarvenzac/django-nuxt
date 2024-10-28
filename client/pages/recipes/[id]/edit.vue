<template>
  <main>
    <div>
      <div>
        <h2>{{ recipe.name }}</h2>
      </div>
      <div>
        <img
          v-if="!preview"
          style="width: 200px"
          :src="recipe.picture" />
        <img
          v-else
          style="width: 200px"
          :src="preview" />
      </div>
      <div>
        <form @submit.prevent="submitRecipe">
          <div>
            <label>Nom de la recette</label>
            <input
              type="text"
              v-model="recipe.name" />
          </div>
          <div>
            <label>Ingrédients</label>
            <input
              type="text"
              v-model="recipe.ingredients" />
          </div>
          <div>
            <label>Photo du plat</label>
            <input
              type="file"
              @change="onFileChange" />
          </div>
          <div>
            <div>
              <div>
                <label>Difficulté</label>
                <select v-model="recipe.difficulty">
                  <option value="Easy">Facile</option>
                  <option value="Medium">Moyen</option>
                  <option value="Hard">Difficile</option>
                </select>
              </div>
            </div>
            <div>
              <div>
                <label> Temps de préparation <small>(minutes)</small> </label>
                <input
                  type="text"
                  v-model="recipe.prep_time" />
              </div>
            </div>
          </div>
          <div>
            <label>Guide de préparation</label>
            <textarea
              v-model="recipe.prep_guide"
              rows="8"></textarea>
          </div>
          <button type="submit">Sauvegarder</button>
        </form>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();

const recipe = ref({
  name: "",
  picture: "",
  ingredients: "",
  difficulty: "",
  prep_time: null,
  prep_guide: "",
});
const preview = ref("");

onMounted(async () => {
  try {
    const recipeData = await $fetch(
      `http://localhost:8000/api/recipes/${route.params.id}/`
    );
    recipe.value = recipeData;
  } catch (error) {
    console.error("Error fetching recipe:", error);
  }
});

function onFileChange(e) {
  const files = e.target.files || e.dataTransfer.files;
  if (!files.length) return;
  recipe.value.picture = files[0];
  createImage(files[0]);
}

function createImage(file) {
  const reader = new FileReader();
  reader.onload = (e) => {
    preview.value = e.target.result;
  };
  reader.readAsDataURL(file);
}

async function submitRecipe() {
  if (!recipe.value) {
    console.error("Recipe data is not available.");
    return;
  }

  const editedRecipe = { ...recipe.value };
  if (
    editedRecipe.picture &&
    editedRecipe.picture.name?.indexOf("http://") !== -1
  ) {
    delete editedRecipe["picture"];
  }

  const formData = new FormData();
  for (let key in editedRecipe) {
    formData.append(key, editedRecipe[key]);
  }

  try {
    await $fetch(`http://localhost:8000/api/recipes/${editedRecipe.id}/`, {
      method: "PATCH",
      body: formData,
    });
    router.push("/recipes/");
  } catch (error) {
    console.error("Error updating recipe:", error);
  }
}
</script>

<style scoped></style>
