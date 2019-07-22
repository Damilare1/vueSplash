<template>
  <div class="rootClass">
    <nav class="navBar">
      <h2 v-if="imgLoaded">
        Search results for
        <span>"{{ searchItem }}"</span>
      </h2>
      <h2 v-else>
        Searching for
        <span>"{{ searchItem }}"</span>
      </h2>
    </nav>
    <div class="wrapper">
      <ul class="imageCardGrid">
        <ImageCard
          v-for="image in images"
          v-on:clicked="onClickChild"
          @loaded="imgLoaded = true"
          :key="image.id"
          :image="image"
        />
      </ul>
    </div>
    <Modal v-if="modal" @close="modal = ''" :imageURL="modal" />
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
    Modal
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
    onClickChild(value) {
      this.modal = value;
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
          order_by: "latest"
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
@import "../utilities/variables";
.rootClass {
  position: relative;
  height: 100%;
}
.wrapper {
  padding: 0;
  position: absolute;
  top: 18vh;
  margin: 0;
  width: 100%;
  display: flex;
  justify-content: center;
  height: fit-content;
  z-index: 1999;
  @media only screen and (max-width: $averageDisplay) {
    max-width: 100%;
    margin: 0;
  }
}
.imageCardGrid {
  justify-content: center;
  width: 100%;
  height: 100%;
  list-style: none;
  margin: 12px 0;
  padding: 0;
  height: 80vh;
  display: grid;
  grid-template-columns: repeat(3, 15%);
  grid-template-rows: repeat(36, 3%);
  grid-column-gap: 20px;
  row-gap: 0;
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

  @media only screen and (min-width: $smallestDisplay) and (max-width: $averageDisplay) {
    grid-template-columns: repeat(2, 40%);
    grid-template-rows: repeat(60, 3%);

    li:nth-of-type(1) {
      grid-row: 1/ 15;
    }
    li:nth-of-type(2) {
      grid-row: 1/ 22;
    }
    li:nth-of-type(3) {
      grid-row: 15/ 37;
    }
    li:nth-of-type(4) {
      grid-row: 22/ 40;
    }
    li:nth-of-type(5) {
      grid-row: 37/ 51;
    }
    li:nth-of-type(6) {
      grid-row: 40/ 60;
    }
  }
  @media only screen and (max-width: $smallestDisplay) {
    grid-template-columns: repeat(1, 80%);
    grid-template-rows: repeat(100, 3%);

    li:nth-of-type(1) {
      grid-row: 1/ 15;
    }
    li:nth-of-type(2) {
      grid-row: 15/ 37;
    }
    li:nth-of-type(3) {
      grid-row: 37/ 59;
    }
    li:nth-of-type(4) {
      grid-row: 59/ 77;
    }
    li:nth-of-type(5) {
      grid-row: 77/ 91;
    }
    li:nth-of-type(6) {
      grid-row: 91/ 100;
    }
  }
}

.navBar {
  display: flex;
  align-items: center;
  padding: 1rem;
  height: 25vh;
  z-index: 1;
  background: #f0f0f0;

  h2 {
    padding-left: 20%;
    font-size: $header;
    width: 100%;

    span {
      color: gray;
    }
    @media screen and (max-width: $smallDisplay) {
      padding: 0;
      font-size: $span;
    }
  }
}
</style>
