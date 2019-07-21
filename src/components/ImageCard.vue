<template>
    <li class="image-card" v-on:click="onClick">
      <img
        :src="image.urls.small"
        :alt="image.description"
        v-on:load="imgLoaded"
        v-show="loaded"
        class="image-card__image"
      />
      <div class="image-card__body">
        <p v-if="image.user.name" class="image-owner">{{image.user.name}}</p>
        <p v-else class="image-owner">Unknown author</p>
        <p v-if="image.user.location" class="image-location">{{image.user.location}}</p>
        <p v-else class="image-location">unknown</p>
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
      this.$emit('loaded',this.loaded);
    }
  }
};
</script>
<style lang="scss">
.image-card {
  position: relative;
  width:  100%;
  margin: 0.5rem;
  display: inline-block;
  border-radius: 5px;
  background-color: grey;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
  &:hover .image-card__image {
    opacity: 0.9;
  }
  &:hover .image-card__body {
    opacity: 1;
  }
  &:empty {
    display: block;
    background-color: black;
  }

  @media only screen and (max-width: 549px) {
    width: 100%;
  }
}
.image-card__image {
  opacity: 1;
  z-index: 13;
  position: relative;
  border-radius: 5px;
  width: 100%;
  height: 100%;
  object-fit: cover;
  img:empty{
    background-color: purple;
  }

  &:empty {
    display: block;
  }
}
.image-card__body {
  position: absolute;
  top: 80%;
  opacity: 0;
  z-index: 1000;
  color: white;
}
.image-title {
  font-weight: bold;
  margin: 0;
}
.image-location,
.image-owner {
  margin: 0;
  padding-left: 10px;
  font-size: 0.8rem;
}
.image-title,
.image-owner {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
</style>