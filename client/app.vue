<template>
  <div>
    <Navigation />
    <NuxtPage />
    <NuxtRouteAnnouncer />
    <!-- <NuxtWelcome /> -->
  </div>
</template>

<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.4s;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
  filter: blur(0.5rem);
}

* {
  margin: 0;
  padding: 0;
}

html {
  height: 100dvh;
  font-family: Inter, sans-serif;
}

body {
  height: 100%;
  padding: 8px;
}

a,
button {
  font-size: 1em;
  font-weight: 400;
  padding: 8px 16px;
  border-radius: 8px;
  border: 1px solid black;
  text-decoration: none;
  color: black;
  background-color: lightgray;
  transition: background-color 300ms ease;
}

a:hover,
button:hover {
  background-color: rgb(83, 192, 88);
}
</style>
