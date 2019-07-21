<template >
  <transition name="modal">
    <div>
      <div class="overlay" @click="$emit('close')">
        <div class="modal">
          <i></i>
          <img :src="url" v-on:load="imgLoaded" v-show="loaded" />
          <div>
            <p>{{name}}</p>
            <p>{{location}}</p>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>

export default {
  
  name: "ImageModal",
  props: ["imageURL"],
  data(){
    return {
      loaded: false,
      url: this.imageURL.urls.small,
      name: this.imageURL.user.name,
      location: this.imageURL.user.location,
    }
  },
  methods: {

    imgLoaded() {
      this.loaded = true;
    },
  },

};
</script>
<style scoped>
.modal {
  width: fit-content;
  height: fit-content;
  margin: 0px auto;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px 3px;
  transition: all 0.2s ease-in;
  font-family: Helvetica, Arial, sans-serif;
}
.fadeIn-enter {
  opacity: 0;
}
.fadeIn-leave-active {
  opacity: 0;
  transition: all 0.2s step-end;
}

.fadeIn-enter .modal,
.fadeIn-leave-active.modal {
  transform: scale(1.1);
}
button {
  padding: 7px;
  margin-top: 10px;
  background-color: green;
  color: white;
  font-size: 1.1rem;
}

img{
  width: 100%;
  height: 100%;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: #00000094;
  z-index: 2999;
  transition: opacity 0.2s ease;
}
</style>