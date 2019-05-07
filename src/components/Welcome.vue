<template>
  <section>
    <div class="image-container">
      <div class='title-small'>
        <h1>Matt Paints!</h1>
        <p>Matt will paint the f*ck out of your house</p>
      </div>

      <!-- houses! -->
      <div class="image-wrapper hidden" v-for="i in getImageCount()" :key="i" :ref="i">
        <img :src="getImagePath(i)" @load="handleLoad(i)">
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
      timeoutID: -1,
      interval: 300, // interval between images
      syncopation: 100
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
        // save id to destroy timeout on unmount
        this.timeoutID = window.setTimeout(() => {
          // reverse loop, start animation on first house and watch it go bananas when it completes
          if (i === this.imageCount) {
            this.animateImage();
          }
          return element.classList.remove("hidden");
        }, this.interval * i);
      }
    },
    animateImage() {
      // save id to destroy timeout on unmount
      this.timeoutID = window.setTimeout(() => {
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
        // next image!
        this.animateImage();
      }, this.interval + this.getSyncopation());
    },
    getSyncopation() {
      return _.random(0, 1) === 1 ? this.syncopation : this.syncopation * -1;
    },

    getImagePath(i) {
      const isLastOfSeries = i % this.imageCount === 0;
      const numberOfSeries = parseInt(i / this.imageCount);

      if (i <= this.imageCount) return `/images/houses/${i}.png`;
      else
        return `/images/houses/${
          isLastOfSeries ? this.imageCount : i % this.imageCount
        }-color-${isLastOfSeries ? numberOfSeries - 1 : numberOfSeries}.png`;
    },
    getImageCount() {
      return this.imageCount * (this.colorVariations + 1);
    }
  },
  beforeDestroy() {
    window.clearTimeout(this.timeoutID);
  }
};
</script>

<style lang="scss" scoped>
@import "../stylesheets/variables";

section {
  display: flex;
  flex-flow: row nowrap;
}

.image-container {
  position: relative;
  width: 66.6%;
  background: linear-gradient(
    -30deg,
    $bg-yellow,
    $bg-blue 60%,
    $bg-pink 10%,
    white
  );
  overflow: hidden;

  .image-wrapper {
    position: absolute;
    bottom: -6px;
    left: 0;
  }
  img {
    width: 100%;
    height: 100%;
    height: 100%;
  }

  .title-small {
    display: none;
  }
}

.content-container {
  position: relative;
  width: 33.3%;
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
  background: azure;
}

h1 {
    font-family: "Sacramento", cursive;
    transform: rotate(-10deg);
    font-size: 4rem;
    margin-bottom: 0;
  }
  p {
    border-bottom: solid 2px red;
  }

  h1, p {
    text-align: center;
    margin: 0 1rem;
  }

@media (max-width: $bp-medium) {
  .image-container {
    width: 100%;
    .title-small {
      width: max-content;
      display: block;
      margin: 25vh auto 0 auto;
    }

    h1 {
      transform: rotate(0deg);
    }
  }
  .content-container {
    display: none;
  } 
}

@media (max-width: $bp-x-small) {
  h1 {
    font-size: 3rem;
  }
  p, h1 {
    width: 75vw;
  }
}
</style>
