<template>
  <div class="Navbar">
    <form @submit.prevent="searchImage()">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        version="1.1"
        id="Capa_1"
        x="0px"
        y="0px"
        width="20"
        height="20"
        viewBox="0 0 612.01 612.01"
        style="enable-background:new 0 0 612.01 612.01;"
        xml:space="preserve"
      >
        <g>
          <g>
            <g id="_x34__4_">
              <g>
                <path
                  d="M606.209,578.714L448.198,423.228C489.576,378.272,515,318.817,515,253.393C514.98,113.439,399.704,0,257.493,0     C115.282,0,0.006,113.439,0.006,253.393s115.276,253.393,257.487,253.393c61.445,0,117.801-21.253,162.068-56.586     l158.624,156.099c7.729,7.614,20.277,7.614,28.006,0C613.938,598.686,613.938,586.328,606.209,578.714z M257.493,467.8     c-120.326,0-217.869-95.993-217.869-214.407S137.167,38.986,257.493,38.986c120.327,0,217.869,95.993,217.869,214.407     S377.82,467.8,257.493,467.8z"
                  data-original="#000000"
                  class="active-path"
                  style="fill:#7A7A7A"
                  data-old_color="#000000"
                />
              </g>
            </g>
          </g>
        </g>
      </svg>
      <input type="text" v-model="search" placeholder="Search free hd images" class="Input__search" />
    </form>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  name: "AppHeader",
  data() {
    return {
      search: "",
    };
  },
  methods: {
    //Emits the search value to Photos component
    searchImage() {
      bus.$emit("searchPhotos", this.search);
      this.search = "";
    },
  },
};
</script>
<style scoped>
.Navbar {
  box-shadow: 0 0 25px 0px rgb(0 0 0 / 40%);
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 9;
  background: #fff;
}
.Navbar form {
  display: flex;
  align-items: center;
  padding: 0.5em 1em;
}
.Input__search {
  font-size: 1.125rem;
  width: 100%;
  padding: 1em 0;
  border: 0;
  margin-left: 0.4em;
}
.Input__search:focus,
.Input__search:hover {
  outline: 0;
}
</style>