<template>
	<main id="app">
		<Landing />
	</main>
</template>
<script>
import Landing from "./components/Landing.vue";

export default {
	name: "app",
	components: {
		Landing
	}
};
</script>
<style lang="scss">
#app {
	font-family: "Avenir", Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}
</style>
