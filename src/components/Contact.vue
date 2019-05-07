<template>
  <section>
    <div>
      <p>CALL ME. ANYTIME.</p>
      <a href="tel:+1231231234" target="_blank" rel="noopener noreferrer">123-123-1234</a>
      <a
        href="mailto:test@test.com"
        target="_blank"
        rel="noopener noreferrer"
      >MattThePainter@painty.pants</a>
    </div>
  </section>
</template>

<script></script>

<style lang="scss" scoped>
@import "../stylesheets/variables";

section {
  background: linear-gradient($bg-blue, $bg-yellow);
  padding: 5rem 0;
  text-align: center;
  min-height: 100%;
}

div {
  margin: 0 auto;
  padding: 0 2rem;
  max-width: 1250px;
}

p {
  font-size: 25vw;
  word-break: break-all;
  margin: 0;
}

a {
  display: block;
  word-break: break-all;
  margin: 5rem 0 0 0;
  font-size: 10vw;
  color: #000;
}

@media (max-width: $bp-medium) {
  p {
    font-size: 20vw;
  }
  a {
    margin-top: 2rem;
  }
}
</style>

