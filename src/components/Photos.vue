<template>
  <div class="Photos">
    <div v-if="isVisible" class="Loader">
      <div class="Loader__spin"></div>
    </div>
    <div v-else>
      <div class="Photos__wrap">
        <!-- <div v-for="(photo,index) in photos" v-bind:key="index">
        <div v-for="item in photo" v-bind:key="item">{{item.id}}</div>
        </div>-->
        <div v-masonry fit-width="true" transition-duration="0.3s" item-selector=".item">
          <div v-for="(photo,index) in photos" v-bind:key="index">
            <div v-for="(item,i) in photo" v-bind:key="i" v-masonry-tile class="item Photo">
              <img class="Photo__img" :alt="item.alt_description" :src="item.urls.small" />
              <span class="Download" @click="downloadImg(item.urls.full,item.description)">
                <svg
                  width="25"
                  height="25"
                  version="1.1"
                  viewBox="0 0 32 32"
                  fill="#767676"
                  aria-hidden="false"
                >
                  <path d="M25.8 15.5l-7.8 7.2v-20.7h-4v20.7l-7.8-7.2-2.7 3 12.5 11.4 12.5-11.4z" />
                </svg>
              </span>
            </div>
          </div>
        </div>
      </div>
      <div class="Photos__cta">
        <button @click="loadPhotos" class="load-more">Load More</button>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import FileSaver from "file-saver";
import axios from "axios";

import { bus } from "../main";
import { VueMasonryPlugin } from "vue-masonry";

Vue.use(VueMasonryPlugin);
export default {
  name: "Photos",
  props: ["search"],
  components: {},
  loading: false,
  data() {
    return {
      photos: [],
      searchText: "random", //Value to be sent default
      page: 1,
      isVisible: false,
    };
  },
  methods: {
    //Load photos initially
    async loadPhotos() {
      this.isVisible = true;
      this.page = this.page + 1;
      const clientId = "caNTMHPjy8crfU5ar0dXLa4E5wr9SjAXXtfs5F2kEd8";
      const url = `https://api.unsplash.com/search/photos?page=${this.page}&query=${this.searchText}&client_id=${clientId}`;
      try {
        const res = await axios.get(url);
        this.photos.push(res.data.results);
        //console.log(this.photos);
      } catch (err) {
        console.log(err);
      }
      this.isVisible = false;
    },
    downloadImg(img, title) {
      FileSaver.saveAs(img, title);
    },
  },
  async created() {
    this.isVisible = true;
    //API call using axios
    const clientId = "caNTMHPjy8crfU5ar0dXLa4E5wr9SjAXXtfs5F2kEd8";
    let url = `https://api.unsplash.com/search/photos?page=${this.page}&query=${this.searchText}&client_id=${clientId}`;
    try {
      const res = await axios.get(url);
      this.photos.push(res.data.results);
      //console.log(this.photos);
    } catch (err) {
      console.log(err);
    }
    //Get the emitted data from header and trigger search function
    bus.$on("searchPhotos", (data) => {
      this.isVisible = true;
      this.photos.length = null;
      this.searchText = data; //Set the search value from header component
      //console.log(this.searchText);
      url = `https://api.unsplash.com/search/photos?page=${this.page}&query=${this.searchText}&client_id=${clientId}`;
      //API call using fetch
      fetch(url)
        .then((res) => res.json())
        .then((data) => {
          this.photos.push(data.results);
        });
      this.isVisible = false;
    });
    this.isVisible = false;
  },
};
</script>
<style scoped>
.Photos {
  height: 100%;
  margin-top: 80px;
}
.Photos__wrap {
  display: flex;
  justify-content: center;
}
.Photo {
  position: relative;
  margin: 1em;
  cursor: pointer;
  transition: all 250ms ease-in-out;
}

.Download {
  position: absolute;
  background: #fff;
  display: flex;
  right: 10px;
  bottom: 10px;
  width: 35px;
  height: 35px;
  opacity: 0;
  justify-content: center;
  border-radius: 5px;
  align-items: center;
}
.Download path {
  color: #767676;
}
.Photo:hover {
  opacity: 0.8;
  transition: opacity 200ms ease-in-out;
}
.Photo:hover .Download {
  opacity: 1;
}
.Photos__cta {
  text-align: center;
  margin: 1em 0;
}
.load-more {
  padding: 1em 1.5em;
  font-size: 1rem;
  background: #ffffff;
  border-radius: 5px;
  border: 1px solid #7a7a7a;
  color: #4c4a4a;
  font-weight: 400;
  cursor: pointer;
}
.load-more:focus {
  outline: none;
}
.Loader {
  background: #fff;
  height: 100%;
  min-height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.Loader__spin {
  width: 55px;
  height: 55px;
  border-radius: 50px;
  border: 5px solid #fff;
  background: transparent;
  border-color: #d9d9d9;
  position: relative;
  box-shadow: 0px 2px 5px -5px rgba(0, 0, 0, 0.8);
}

.Loader__spin::before {
  content: "";
  width: 55px;
  height: 55px;
  border-radius: 50%;
  border: 5px solid;
  border-color: #3f72af transparent transparent transparent;
  background: black;
  position: absolute;
  top: -5px;
  left: -5px;
  background: transparent;
  animation: rotate 1.12s cubic-bezier(0.64, 0.5, 0.01, 0.55) infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>