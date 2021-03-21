<template>
  <div class="photo-block">
    <h3 class="title" v-if="!hideTitle">{{photo.title}}</h3>
    <div class="photo-block-body">
      <div class="icon">
        <svg viewBox="0 0 13.229166 13.229167" :id="'photo-id-' + photo.id">
          <path :class="{'star-active' : photo.inBookmarks}"
                :photo-id="photo.id"
                :album-id="photo.albumId"
                fill="#b3b3b3"
                d="M 101.06769,73.924255 80.187146,58.910306 59.431821,74.096887 67.258493,49.598745 46.401448,34.55217 72.119139,34.425435 79.984101,9.9395592 88.051837,34.359374 113.7697,34.272846 93.03814,49.491856 Z"
                transform="matrix(0.19637095,0,0,0.20619884,-9.1118964,-2.0495256)" />
        </svg>
      </div>
      <div class="photo">
        <img :src="photo['thumbnailUrl']" :title="photo.title" :alt="photo.title" @click="openFullPicture = true">
      </div>
    </div>
    <div class="full-photo-block" v-if="openFullPicture">
      <div class="full-photo">
        <span class="close-button" @click="openFullPicture = false" title="Закрыть">X</span>
        <img :src="photo['url']" :title="photo.title" :alt="photo.title">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Photo",
  props: ['photo', 'hideTitle'],
  data: () => ({
    openFullPicture : false,
    isAdded: false
  }),
  methods : {
    handleClick(element){
      element.addEventListener("click", () => {
        if (!this.photo.inBookmarks)
          this.addToBookmark(element.children[0]);
        else
          this.deleteFromBookmark(element.children[0]);
      })
    },
    addToBookmark(element){
      this.$emit("update-bookmarks", element, true);
    },
    deleteFromBookmark(element){
      this.$emit("update-bookmarks", element, false);
    },
  },
  mounted() {
    const star = document.querySelector(".icon #photo-id-" + this.photo.id);
    this.handleClick(star);
  }
}
</script>

<style scoped>
  .photo-block {
    cursor: default;
    width: 100%;
    margin: 15px;
  }
  .title {
    display: block;
    text-align: left;
  }
  .photo-block-body {
    display: inline-flex;
    width: 100%;
  }
  .icon {
    max-width: 25px;
    display: block;
  }
  .icon svg {
    width: 25px;
    height: 25px;
    cursor: pointer;
  }
  .star-active {
    fill: yellow !important;
  }
  .photo {
    display: inline-block;
    margin-left: 25px;
  }
  .photo img {
    cursor: pointer;
  }
  .full-photo-block {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vh;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(2px);
    z-index: 1;
  }
  .full-photo {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .close-button{
    cursor: pointer;
    position: absolute;
    top: 25px;
    left: calc(100% - 35px)
  }
</style>