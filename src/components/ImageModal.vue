<template >
  <transition name="modal">
    <div>
      <div class="overlay" @click="$emit('close')">
        <div class="modal">
          <i class="clearButton">
            <CloseIcon fillColor="white" :size="10" />
          </i>
          <div class="image">
            <div v-if="!loaded" class="loadingPlaceholder" />
            <img :src="url" v-on:load="imgLoaded" v-show="loaded" />
          </div>
          <div class="details">
            <p v-if="name" class="name">{{name}}</p>
            <p v-else class="name">Author unknown</p>
            <p v-if="location" class="location">{{location}}</p>
            <p v-else class="location">Location unspecified</p>
          </div>
          
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import CloseIcon from "vue-material-design-icons/Close";

export default {
  name: "ImageModal",
  props: ["imageURL"],
  components: {
    CloseIcon
  },
  data() {
    return {
      loaded: false,
      url: this.imageURL.urls.regular,
      name: this.imageURL.user.name,
      location: this.imageURL.user.location
    };
  },
  methods: {
    imgLoaded() {
      this.loaded = true;
    }
  }
};
</script>
<style scoped lang="scss">
@import "../utilities/variables";
.loadingPlaceholder {
  width: 300px;
  height: 300px;
  animation: animationLoader 1s infinite ease-in-out;
}
.modal {
  position: relative;
  margin: 0px auto;
  width: fit-content;
  max-height: 700px;
  max-width: 300px;
  background-color: #fff;
  border-radius: $radius;
  pointer-events: none;
  box-shadow: 0 2px 8px 3px;
  transition: all 3s ease-in;
  font-family: Helvetica, Arial, sans-serif;
  @media only screen and (max-width: $averageDisplay) {
    max-height: 80%;
    max-width: 80%;
  }
  .image {
    border-radius: $radius $radius 0 0;
    width: auto;
    height: auto;
    max-height: 90%;
    max-width: 100%;
    overflow: hidden;
    transition: 4s all;
    @media only screen and (max-width: $smallDisplay) and(orientation: portrait){
    max-height: 300px;
    max-width: 270px;
    }
   @media only screen and (max-width: $averageDisplay) and(orientation: landscape){
    max-height: 200px;
    max-width: 500px;
    }

  }
  img {
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: 100%;
  }
}
p {
  padding: 0;
  margin: 0;
}

.name {
  text-transform: capitalize;
  font-size: $modalNameP;
}
.location {
  font-size: $modalLocationP;
  text-transform: capitalize;
}
.details {
  margin: 10px;
}

.clearButton {
  position: absolute;
  left: 105%;
  bottom: 100%;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  height: 100%;
  background: #00000094;
  z-index: 2999;
  transition: blowUpModal 1s ease;
}
@keyframes blowUpModal {
  0% {
    transform:scale(0);
  }
  100% {
    transform:scale(1);
  }
}

@keyframes blowUpModalTwo {
  0% {
    transform:scale(1);
    opacity:1;
  }
  100% {
    transform:scale(0);
    opacity:0;
  }
}

</style>