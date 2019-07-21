<template>
  <li class="imageCard" v-on:click="onClick">
    <img
      :src="image.urls.small"
      :alt="image.description"
      v-on:load="imgLoaded"
      v-show="loaded"
      class="imageCardImg"
    />
    <div v-if="loaded" class="cardBody">
      <div  class="detailCard">
        <p v-if="image.user.name" class="name">{{image.user.name}}</p>
        <p v-else class="name">Unknown author</p>
        <p v-if="image.user.location" class="location">{{image.user.location}}</p>
        <p v-else class="location">Location unspecified</p>
      </div>
    </div>
    <div v-else class="cardBodyLoading">
      <div class="loadingDetail">
        <p class="loadingName"></p>
        <p class="loadingLocation"></p>
      </div>
    </div>
  </li>
</template>
<script>
export default {
  name: "ImageCard",
  props: ["image"],

  data() {
    return {
      loaded: false
    };
  },
  methods: {
    onClick() {
      this.$emit("clicked", this.image);
    },
    imgLoaded() {
      this.loaded = true;
      this.$emit("loaded", this.loaded);
    }
  }
};
</script>
<style lang="scss">
.imageCard {
  position: relative;
  width: 100%;
  margin: 0.5rem 0;
  display: inline-block;
  border-radius: 5px;
  background-color: lightgrey;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
  font-family: Helvetica, Arial, sans-serif;
  &:hover .cardBody {
    opacity: 1;
  }

  @media only screen and (max-width: 549px) {
    width: 100%;
  }
}
.imageCardImg {
  opacity: 1;
  z-index: 13;
  position: relative;
  border-radius: 5px;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.cardBody {
  position: absolute;
  display: flex;
  text-transform: capitalize;
  align-items: flex-end;
  height: 100%;
  width: 100%;
  background: rgb(0, 0, 0);
  background: linear-gradient(0deg, rgba(0, 0, 0, 1) 7%, rgba(0, 0, 0, 0) 42%);
  top: 0%;
  opacity: 0;
  z-index: 1000;
  color: white;
  .detailCard {
    margin: 10px;
    .name {
      padding: 0;
      margin: 0;
      font-size: 15px;
      text-transform: capitalize;
    }
    .location {
      padding: 0;
      margin: 0;
      font-size: 10px;
      text-transform: capitalize;
    }
  }
}
.cardBodyLoading {
  position: absolute;
  display: flex;
  align-items: flex-end;
  height: 100%;
  width: 100%;
  background: transparent;
  top: 0%;
  opacity: 1;
  z-index: 1000;
  
  .loadingDetail{
  margin:10px;
  width: 100%;
}
.loadingLocation {
  height: 10px;
  width: 40%;
  margin: 0;
  animation: animationLoader 1s infinite ease-in-out;
}
.loadingName {
  height: 15px;
  width: 60%;
  margin-bottom: 2px;
  animation: animationLoader 1s infinite ease-in-out;
}
}

@keyframes animationLoader {
  0% {
    background-color: rgba(128, 128, 128, 0.1);
  }
  50% {
    background-color: rgba(61, 61, 61, 0.3);
  }
  100% {
    background-color: rgba(15, 15, 15, 0.1);
  }
}
</style>