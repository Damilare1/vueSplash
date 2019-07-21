<template>
  <div>
    <nav class="navbar">
      <h2 v-if="imgLoaded">Search results for <span>"{{searchItem}}"</span></h2>
      <h2 v-else>Searching for <span>"{{searchItem}}"</span></h2>

    </nav>
    <div class="wrapper">
      <ul class="image-card-grid">
        <ImageCard v-for="image in images" v-on:clicked="onClickChild" @loaded="imgLoaded=true" :key="image.id" :image="image" />
      </ul>
    </div>
    <Modal v-if="modal" @close="modal=''" :imageURL="modal"/>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import ImageCard from "@/components/ImageCard";
import Modal from "@/components/ImageModal";

export default {
  name: "home",
  components: {
    ImageCard,
    Modal,
  },
  data() {
    return {
      query: "",
      images: [],
      loading: false,
      imgLoaded: false,
      searchItem: this.$route.params.searchItem,
      modal: ""
    };
  },
  methods: {
    search() {
      (this.loading = true),
        this.fetchData(this.query).then(response => {
          this.images = response.data.results;
          this.loading = false;
        });
    },
    onClickChild(value){
      this.modal=value;
    },
    fetchData(queryParam) {
      return axios({
        method: "get",
        url: "https://api.unsplash.com/search/photos/",
        params: {
          client_id:
            "4d46da2d74d993bb6e21e900bc8bec408fd46c87d294ddfcfc4ce153995a30a3",
          query: queryParam,
          page: 1,
          per_page: 6,
          order_by: "latest",
        }
      });
    }
  },
  beforeMount() {
    (this.loading = true),
      this.fetchData(this.searchItem).then(response => {
        this.images = response.data.results;
        this.loading = false;
      });
  }
};
</script>
<style lang="scss">
.screen-reader-only {
  height: 1px;
  width: 1px;
  position: absolute;
  left: -100000px;
}
.wrapper {
  position: absolute;
  top: 18%;
  margin: 0 auto;
  width: 100%;
  height: fit-content;
  z-index: 1999;
  @media only screen and (max-width: 799px) {
    max-width: 100%;
    margin: 0 0;
  }
}
.image-card-grid {
  justify-content: center;
  width: fit-content;
  height: fit-content;
  list-style: none;
  margin: 0.5rem 0;
  padding: 0;
  height: 80vh;
  display: grid;
  grid-template-columns: repeat(3, 15%);
  grid-template-rows: repeat(36, 3%);
  grid-column-gap: 20px;
  row-gap: 0;
  @media only screen and (max-width: 800px){
      grid-template-columns: repeat(3, 1fr);
  }
  li:nth-of-type(1) {
    grid-row: 1/ 15;
  }
  li:nth-of-type(2) {
    grid-row: 1/ 22;
  }
  li:nth-of-type(3) {
    grid-row: 1/ 16;
  }
  li:nth-of-type(4) {
    grid-row: 15/ 33;
  }
  li:nth-of-type(5) {
    grid-row: 22/ 36;
  }
  li:nth-of-type(6) {
    grid-row: 16/ 36;
  }

}

.navbar {
  position: relative;
  display: flex;
  align-items: center;
  padding: 1rem;
  height: 25vh;
  z-index: 1;
  background: #f0f0f0;
  
  h2{
    padding-left: 20%;
    font-size: 30px;
    width:100%;

    span{
      color: gray;
    }
  }
}

</style>
