<template>
  <section>
    <p>CALL ME. ANYTIME.</p>
    <a href="tel:+1231231234">123-123-1234</a>
  </section>
</template>

<script></script>

<style lang="scss" scoped>
@import "../stylesheets/variables";

section {
  background: linear-gradient($bg-blue, $bg-yellow);
  min-height: 100vh;
  height: unset;
  padding: 5rem 0;
  text-align: center;
}

p {
  font-size: 25vw;
  word-break: break-all;
  margin: 0;
}

a {
  font-size: 15vw;
  color: #000;
}
</style>

