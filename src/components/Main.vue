<template>
  <div class="main">
    <div class="main__first">
      <input type="text" v-model="inputImage">
      <button @click="setImage">Ok</button>
    </div>
    <div class="main__second">
      <img v-if="image != null" :style="{height: imageHeight, width: imageWidth}" :src="image" alt="image">
    </div>
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      image: null,
      inputImage: "",
      imageWidth: 0,
      imageHeight: 0,
      innerWidth: 0,
      innerHeight: 0
    }
  },
  created() {
    window.addEventListener("resize", this.myEventHandler);
  },
  destroyed() {
    window.removeEventListener("resize", this.myEventHandler);
  },
  methods: {
    myEventHandler(e) {
      this.innerWidth = e.target.innerWidth
      this.innerHeight = e.target.innerHeight

      if (this.imageHeight > this.imageWidth) {
        const aspectRatio = 4 / 5
        this.imageHeight = this.innerHeight / 2 + "px"
        this.imageWidth = this.innerHeight / 2 * aspectRatio + "px"
      } else if (this.imageHeight < this.imageWidth) {
        const aspectRatio = 16 / 9
        this.imageHeight = this.innerWidth / 2 / aspectRatio + "px"
        this.imageWidth = this.innerWidth / 2 + "px"
      }
    },
    async setImage() {
      const newImg = new Image()
      newImg.src = this.inputImage
      newImg.onload = () => {
        const inputWidth = newImg.naturalWidth;
        const inputHeight = newImg.naturalHeight;

        if (inputHeight > inputWidth) {
          const aspectRatio = 4 / 5
          this.imageHeight = this.innerHeight / 2 + "px"
          this.imageWidth = this.innerHeight / 2 * aspectRatio + "px"
        } else if (inputHeight < inputWidth) {
          const aspectRatio = 16 / 9
          this.imageHeight = this.innerWidth / 2 / aspectRatio + "px"
          this.imageWidth = this.innerWidth / 2 + "px"
        }

        this.image = this.inputImage
      }
    }
  }
}
</script>

<style scoped>
  .main {
    display: flex;
    justify-content: space-evenly;
  }

  .main__first {
    display: flex;
    height: fit-content;
    justify-content: center;
  }

  .main__second {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  @media screen and (max-width: 767px) {
    .main {
      flex-direction: column;
    }

    .main__first {
      margin-bottom: 10px;
    }
  }
</style>
