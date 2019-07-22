<template>
  <div class="divClass">
    <nav class="navBar">
      <form class="searchBar" @submit.prevent>
        <label>
          <i><MagnifyIcon fillColour="grey" :size="12"/></i>
          <input
            v-model="query"
            type="text"
            placeholder="Search for photo"
            class="searchBarInput"
            v-on:keyup.enter="search"
          />
        </label>
      </form>
    </nav>
    <div class="wrapper">
      <ul class="imageCardGrid">
        <ImageCard
          v-for="image in images"
          v-on:clicked="onClickChild"
          :key="image.id"
          :image="image"
        />
      </ul>
    </div>
    <Modal v-if="modal" @close="modal=''" :imageURL="modal" />
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import ImageCard from "@/components/ImageCard";
import Modal from "@/components/ImageModal";
import MagnifyIcon from "vue-material-design-icons/Magnify";

export default {
  name: "home",
  components: {
    ImageCard,
    MagnifyIcon,
    Modal
  },
  data() {
    return {
      query: "",
      images: [],
      loading: false,
      modal: ""
    };
  },
  methods: {
    search() {
      this.$router.push("/" + this.query);
    },
    onClickChild(value) {
      this.modal = value;
    },
    fetchData(queryParam) {
      return axios({
        method: "get",
        url: "https://api.unsplash.com/search/photos/",
        params: {
          client_id: process.env.VUE_APP_ClientId,
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
      this.fetchData("africans").then(response => {
        this.images = response.data.results;
        this.loading = false;
      });
  }
};
</script>
<style lang="scss">
@import "../utilities/variables";
.searchBar {
  width: 100%;
  display: flex;
  margin:0;
  padding:0;
  align-items: center;
  justify-content: center;
  label {
    align-items: center;
    display: inline-flex;
    width:65%;
    i {
      position: absolute;
      margin-left: 10px;
      margin-top: 1px;
    }
    input {
      width:100%;
      padding-left: 40px;
    }
  }
  @media only screen and (max-width: $smallDisplay) {
    width: 100%;
    label {
      width: 80%;
    }
  }
}
.searchBarInput {
  padding: 0.5rem 1rem;
  border-radius: 7px;
  font-size: 1rem;
  border: 1px solid white;
  min-width: 300px;
  width: 800px;
  &:focus {
    border-color: grey;
    outline: none;
  }
  @media only screen and (max-width: $smallDisplay) {
    min-width: 0;
    width: 100%;
  }
}

</style>
