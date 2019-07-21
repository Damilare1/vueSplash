<template>
  <div class="divClass">
    <nav class="navbar">
      <form class="searchbar" @submit.prevent>
        <label>
          <i>
            <MagnifyIcon :size="16" />
          </i>
          <input
            v-model="query"
            type="text"
            placeholder="Search for photo"
            class="searchbar-input"
            v-on:keyup.enter="search"
          />
        </label>
      </form>
    </nav>
    <div class="wrapper">
      <ul class="image-card-grid">
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
      this.fetchData("africans").then(response => {
        this.images = response.data.results;
        this.loading = false;
      });
  }
};
</script>
<style lang="scss">
.divClass{
  background-color: pink;
}

.wrapper {
  position: absolute;
  background-color: blue;
  top: 18%;
  width: 100%;
  height: fit-content;
  z-index: 1999;
 
 @media screen and (max-width: 768px) {
  margin: 0;
  padding: 0;
 }

}
.image-card-grid {
  justify-content: center;
  background-color: red;
  width: 100%;
  height: fit-content;
  list-style-type: none;
  margin:  0;
  padding: 0;
  height: fit-content;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(36, 3%);
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
  
}

.divClass{
  outline-offset: 0;
}

.navbar {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  height: 25vh;
  z-index: 1;
  background: #f0f0f0;
}
.searchbar {
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
      font-size: 2px;
    }
    input {
      width:100%;
      padding-left: 40px;
    }
  }
  @media only screen and (max-width: 549px) {
    width: 100%;
    label {
      width: 80%;
    }
  }
}
.searchbar-input {
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
  @media only screen and (max-width: 549px) {
    min-width: 0;
    width: 100%;
  }
}

</style>
