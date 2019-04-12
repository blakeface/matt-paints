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
			timeoutIDs: [],
			interval: 500, // interval between images
			syncopation: 250
		};
	},
	methods: {
		handleLoad(i) {
			// add 1 to account for original image
			if (i === this.imageCount * (this.colorVariations + 1)) {
				// all images are loaded! remove css hidden class
				this.showImages();
			}
		},
		showImages() {
			// only remove natural images
			for (let i = this.imageCount; i > 0; i--) {
				// get element from $refs
				const element = this.$refs[i] ? this.$refs[i][0] : null;
				const id = window.setTimeout(() => {
					// last image, animate shit
					if (i === this.imageCount) {
						this.showBackgroundImages();
						this.initImageAnimation();
					}
					return element.classList.remove("hidden");
				}, this.interval * i);

				// save id to destroy timeout on unmount
				this.timeoutIDs.push(id);
			}
		},
		initImageAnimation() {
			// animate 50 times
			for (var i = 100; i >= 0; i--) {
				const id = window.setTimeout(() => {
					// get random image, layer, and  array of image variations
					const imageIndex = _.random(1, this.imageCount);
					const layerIndex = _.random(0, this.colorVariations);
					const imageArray = Array.from(
						{ length: this.colorVariations + 1 },
						(val, i) => imageIndex + this.imageCount * i
					);
					// hide all except imageIndex
					imageArray.forEach((refIndex, i) => {
						if (i === layerIndex) {
							this.$refs[refIndex][0].classList.remove("hidden");
						} else {
							this.$refs[refIndex][0].classList.add("hidden");
						}
					});
				}, this.interval + this.getSyncopation() * i);
				// save id for when component is destroyed
				this.timeoutIDs.push(id);
			}
		},
		getSyncopation() {
			return _.random(0, 1) === 1
				? this.interval / 2
				: this.interval / 2 - 1;
		},
		showBackgroundImages() {
			// place to load anxillary images
		},
		getImagePath(i) {
			const isLastOfSeries = i % this.imageCount === 0;
			const numberOfSeries = parseInt(i / this.imageCount);

			if (i <= this.imageCount) return `/images/${i}.png`;
			else
				return `/images/${
					isLastOfSeries ? this.imageCount : i % this.imageCount
				}-color-${
					isLastOfSeries ? numberOfSeries - 1 : numberOfSeries
				}.png`;
		},
		getImageCount() {
			return this.imageCount * (this.colorVariations + 1);
		}
	},
	beforeDestroyed() {
		// kill any remaining timouts
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
