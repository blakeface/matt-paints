<template>
	<section>
		<div class="image-container">
			<div
				class="image-wrapper hidden"
				v-for="i in getImageCount()"
				:key="i"
				:ref="i"
			>
				<img :src="getImagePath(i)" @load="handleLoad(i)" />
			</div>
		</div>
		<div class="content-container">
			<h1>Matt Paints!</h1>
			<p>Matt will paint the f*ck out of your house</p>
		</div>
	</section>
</template>

<script>
import _ from "lodash";

export default {
	data() {
		return {
			imageCount: 7,
			colorVariations: 2,
			colorIndex: 0,
			timeoutIDs: []
		};
	},
	methods: {
		handleLoad(i) {
			// add 1 to account for original image
			if (i === this.imageCount * (this.colorVariations + 1)) {
				this.showImages();
			}
		},
		showImages() {
			console.log("loading image");
			_.forIn(this.$refs, (val, key) => {
				console.log(val, key);
				const id = window.setTimeout(
					() => val[0].classList.remove("hidden"),
					500 * key
				);
				this.timeoutIDs.push(id);
			});
		},
		getImagePath(i) {
			const isFinalOfSeries = i % this.imageCount === 0;

			if (i <= this.imageCount) return `/images/${i}.png`;
			else
				return `/images/${
					isFinalOfSeries ? this.imageCount : i % this.imageCount
				}-color-${
					isFinalOfSeries
						? parseInt(i / this.imageCount) - 1
						: parseInt(i / this.imageCount)
				}.png`;
		},
		getColor(i) {
			return this.colors[parseInt(i / this.imageCount)];
		},
		getImageCount() {
			console.log(
				this.imageCount,
				this.colorVariations,
				this.imageCount * (this.colorVariations + 1)
			);
			return this.imageCount * (this.colorVariations + 1);
		}
	},
	destroyed() {
		this.timeoutIDs.forEach(id => window.clearTimeout(id));
	}
};
</script>

<style scoped lang="scss">
// variables
$aqua: hsla(167, 100, 69, 1);
$yellow: hsla(56, 100, 66, 1);
$red: hsla(350, 100, 60, 1);
$teal: hsla(181, 81, 45, 1);
$purple: hsla(316, 100, 60, 1);
$orange: hsla(23, 100, 61, 1);

$bg_pink: hsla(351, 100, 90, 0.6);

section {
	width: 100%;
	height: 100vh;
	display: flex;
	flex-flow: row nowrap;
}

.image-container {
	position: relative;
	width: 50%;
	background: $bg_pink;

	.image-wrapper {
		position: absolute;
		bottom: 15%;
		left: 0;

		img {
			width: 100%;
			height: 100%;
			height: 100%;
		}
	}
}

.content-container {
	position: relative;
	width: 50%;
	display: flex;
	flex-flow: column nowrap;
	align-items: center;
	justify-content: center;
	background: azure;

	h1 {
		font-family: "Sacramento", cursive;
		transform: rotate(-10deg);
		font-size: 4rem;
		margin-bottom: 0;
	}
	p {
		border-bottom: solid 2px red;
		font-family: "Josefin Slab", serif;
	}
}
</style>
