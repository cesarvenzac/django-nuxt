<template>
  <header>
    <h1>Mes recettes</h1>
    <p>Site pour créer et modifier des recettes de cuisine.</p>
    <span>Fait avec Django et Nuxt</span>
  </header>
</template>

<script>
export default {
  head() {
    return {
      title: "Mes recettes",
    };
  },
};
</script>

<style>
header {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
</style>
