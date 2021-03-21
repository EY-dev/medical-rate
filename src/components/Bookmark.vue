<template>
  <div class="bookmarks-block">
    <list-photos :photos="photos" :albumId="0" :hideTitle="false" @update-bookmarks="updateBookmarks"/>
  </div>
</template>

<script>

import ListPhotos from "@/components/ListPhotos";
export default {
  name: "Bookmark",
  components: {ListPhotos},
  data: () => ({
    photos : []
  }),
  methods: {
    getPhotos(){
      const allData = JSON.parse(localStorage.getItem("bookmarks"));
      if (allData)
        this.photos[0] = allData.filter(elem => elem !== null);
      else
        this.photos[0] = [];
      this.photos = JSON.parse(JSON.stringify(this.photos));
    },
    updateBookmarks(element, value){
      if (!value){
        const photoId = parseInt(element.getAttribute('photo-id'))
        let bookmarks = JSON.parse(localStorage.getItem("bookmarks"))
        const index = bookmarks.findIndex( elem => elem && elem.id === photoId)
        bookmarks[index] = null;
        localStorage.setItem("bookmarks", JSON.stringify(bookmarks));
        this.getPhotos();
      }
    }
  },
  mounted() {
    this.getPhotos();
  }

}
</script>

<style scoped>
.star-active {
  fill: yellow !important;
}
</style>