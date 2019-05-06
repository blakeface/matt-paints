<template>
  <section>
    <canvas id="paper-canvas-about" resize="true"></canvas>
    <div class="frame">
      <p>I've painted over 1 million houses, but I won't stop there. I've painted boats, spaceships, exotic fruits, dolphins, your friend's chihuahua. Shit, I'll even paint your mom.</p>
    </div>
  </section>
</template>

<script>
import paper from "paper";
import { last, random } from "lodash";

export default {
  data() {
    return {
      paths: [],
      colors: ["#E64A7B", "#FC44DB", "#D441F2", "#F26541", "#FC4E44"],
      selectedColor: 0
    };
  },

  methods: {
    setColor() {
      this.selectedColor = random(0, this.colors.length - 1);
    },

    createPath() {
      // create path
      const path = new Path({
        strokeColor: this.colors[this.selectedColor],
        strokeWidth: 150,
        strokeCap: "round"
      });

      path.strokeColor.alpha = 0.3;

      // put in array of paths
      this.paths.push(path);
    }
  },

  created() {
    paper.install(window);
  },

  mounted() {
    this.paper = paper.setup("paper-canvas-about");
    const $frame = document.querySelector(".frame");

    // create trailing paintbrush
    this.createPath();

    this.paper.view.onMouseMove = e => {
      const targetPath = last(this.paths);
      targetPath.add(e.point);
      targetPath.smooth({ type: "continuous" });
    };

    // click
    this.paper.view.onMouseDown = e => {
      // set color and create new path
      this.setColor();
      this.createPath();
    };
    $frame.addEventListener("click", () => {});
  }
};
</script>

<style scoped lang="scss">
@import "../stylesheets/variables";

%flex-center {
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
}

section {
  @extend %flex-center;
  position: relative;
}
canvas {
  // match global section style
  width: 100%;
  min-height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
}

.frame {
  $border: 1rem solid #000;
  $width-height: 66vh;
  $border-position: -3rem;

  min-height: 100vh;

  border: $border;
  z-index: 5;
  width: $width-height;

  position: relative;

  padding: $border-position * -0.5;
  text-align: center;
  @extend %flex-center;
  p {
    max-width: 50%;
    font-size: 2rem;
  }

  &:before {
    content: " ";
    position: absolute;
    z-index: -1;
    top: $border-position;
    left: $border-position;
    right: $border-position;
    bottom: $border-position;
    border: $border;
  }
}
</style>
