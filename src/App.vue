<template>
  <main id="app">
    <Welcome/>
    <About/>
    <Contact/>
  </main>
</template>
<script>
import Welcome from "./components/Welcome.vue";
import About from "./components/About.vue";
import Contact from "./components/Contact.vue";

export default {
  name: "app",
  components: {
    Welcome,
    About,
    Contact
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  margin: 0;
}

// global classes
.hidden {
  display: none;
}

section {
  width: 100%;
  height: 100vh;
}

p,
a {
  font-family: "Josefin Slab", serif;
}
</style>
