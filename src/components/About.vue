<template>
  <section>
    <canvas id="paper-canvas-about" resize="true"></canvas>
    <div class="frame">
      <p>I've painted over 1 million houses, but I won't stop there. I've painted boats, spaceships, exotic fruits, dolphins, your friend's chihuahua. Shit, I'll even paint your mom.</p>
    </div>
    <svg height="512pt" viewBox="0 -96 512 512" width="512pt" xmlns="http://www.w3.org/2000/svg"><path d="m30 247.074219c0-10.957031 4.269531-21.265625 12.019531-29.019531l17.507813-17.507813c26.539062-26.515625 7.710937-72.011719-29.828125-72.011719h-14.699219c-8.285156 0-15 6.714844-15 15 0 8.28125 6.714844 15 15 15h14.699219c10.863281 0 16.277343 13.144532 8.613281 20.800782l-17.511719 17.507812c-13.414062 13.425781-20.800781 31.261719-20.800781 50.238281 0 39.976563 32.375 72.117188 72.117188 72.117188h10.414062c-6.785156-8.761719-11.71875-19.011719-14.234375-30.179688-21.835937-1.945312-38.296875-20.398437-38.296875-41.945312zm0 0"/><path d="m444.890625 128.78125-29.289063-24.40625v-25.109375c0-43.707031-35.5625-79.265625-79.285156-79.265625-41.835937 0-79.25 33.753906-79.25 79.28125 0 43.769531 35.484375 79.25 79.25 79.25 8.285156 0 15 6.71875 15 15 0 8.285156-6.714844 15-15 15-60.335937 0-109.25-48.910156-109.25-109.25 0-3.132812.140625-6.238281.398438-9.308594-75.109375 19.640625-131.066406 87.886719-131.066406 169.945313v32.164062c0 25.980469 21.136718 47.117188 47.117187 47.117188h321.367187c25.980469 0 47.117188-21.136719 47.117188-47.117188v-.023437c0-55.519532-24.460938-107.742188-67.109375-143.277344zm-44.289063 77.953125c-8.28125 0-15-6.726563-15-15 0-8.28125 6.71875-15 15-15 8.277344 0 15 6.71875 15 15 0 8.273437-6.722656 15-15 15zm0 0"/></svg>
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
        strokeWidth: 175,
        strokeCap: "round"
      });

      path.strokeColor.alpha = 0.3;

      // put in array of paths
      this.paths.push(path);
    },

    handleClick() {
      // set color and create new path
      this.setColor();
      this.createPath();
    },

  },

  created() {
    paper.install(window);
  },

  mounted() {
    this.paper = paper.setup("paper-canvas-about");
    const $frame = document.querySelector(".frame");
    const $$p = document.querySelectorAll("p");
    const $svg = document.querySelector('svg')

    // create trailing paintbrush
    this.createPath();

    this.paper.view.onMouseMove = e => {
      const targetPath = last(this.paths);
      targetPath.add(e.point);
      targetPath.smooth({ type: "continuous" });
    };

    // click
    this.paper.view.onMouseDown = e => {
      this.handleClick();
    };
    $frame.addEventListener("click", () => this.handleClick());
    $$p.forEach(p => p.addEventListener("click", () => this.handleClick()));

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

  border: $border;
  z-index: 5;
  width: $width-height;
  min-height: $width-height;

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

svg {
  $position: 1rem;
  $height: 2.5rem;

  position: absolute;
  bottom: $position;
  right: $position;
  height: $height;
  width: $height;

  animation: shake 2s linear infinite alternate;
}

@keyframes shake {
  0%{
    transform: rotate(-5deg)
  }

  5% {
    transform: rotate(5deg)
  }

  10% {     
    transform: rotate(-5deg)
  }
  15% {
    transform: rotate(0deg) 
  }
  100% {
    transform: rotate(0deg)
  }
}

@media (max-width: $bp-small) {
  .frame {
    width: 60%;

    p {
      max-width: 100%;
    }
  }
}

@media (max-width: $bp-x-small) {
  .frame {
    border: none;
    &:before {
      border: none;
      position: relative;
    }

    p { 
      font-size: 1.5rem;
    }
  }
}
</style>
