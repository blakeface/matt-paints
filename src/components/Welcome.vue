<template>
	<section>
		<div class="image-container">
			<div class="image-wrapper hidden" v-for="i in imageCount" :key="i" :ref="i">
				<img :src="`/images/${i}.png`" @load="handleLoad(i)">
			</div>
		</div>
		<div class="content-container">
			<h1>test header</h1>
			<p>test paragraph</p>
		</div>
	</section>
</template>

<script>
	import _ from "lodash";

	export default {
		data() {
			return {
				imageCount: 7,
				intervalId: null
			};
		},
		methods: {
			handleLoad(i) {
				if (i === this.imageCount) {
					this.showImages();
				}
			},
			showImages() {
				_.forIn(this.$refs, (val, key) => {
					setTimeout(() => val[0].classList.remove("hidden"), 500 * key);
				});
			},
			animateImages() {}
		},
		destroyed() {
			window.clearInterval(this.intervalId);
		}
	};
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

	.image-wrapper {
		position: absolute;
		bottom: 0;
		left: 0;

		img {
			width: 100%;
			height: 100%;
			height: 100%;
		}
	}

	.content-container {
		position: relative;
		width: 50%;
		display: flex;
		flex-flow: column nowrap;
		align-items: center;
		background: azure;
	}
</style>
