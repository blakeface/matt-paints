<template>
	<section>
		<div class="image-container">
			<div class="image-wrapper" v-for="i in 7" :key="i">
				<img :src="`/images/${i}.png`">
			</div>
		</div>
		<div class="content-container">
			<h1>test header</h1>
			<p>test paragraph</p>
		</div>
	</section>
</template>
<script>
	export default {};
</script>
<style scoped lang="scss">
	section {
		width: 100%;
		height: 100vh;
		display: flex;
		flex-flow: row nowrap;
	}

	.image-container {
		position: relative;
		width: 50%;
	}
	.content-container {
		position: relative;
		width: 50%;
		display: flex;
		flex-flow: column nowrap;
		align-items: center;
		background: azure;
	}

	.image-wrapper {
		position: absolute;
		top: 0;
		left: 0;
	}
</style>
