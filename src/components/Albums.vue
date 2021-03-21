<template>
  <div class="albums" :id="'user-id-' + userid">
    <div v-for="(album, index) in albums" :key="index" :album-id="album.id">
      <div class="album-name" @click="togglePictures(album.id)">
        <span class="arrow">></span> {{album.title}}
      </div>
      <list-photos v-if="pictureAccess[album.id]" :hideTitle="true" :albumId="album.id" :photos="photos" @update-bookmarks="updateBookmarks"/>
    </div>
  </div>
</template>

<script>
import ListPhotos from "@/components/ListPhotos";
export default {
  name: "Albums",
  components: {ListPhotos},
  props:["userid", "albums"],
  data: () => ({
    pictureAccess : [],
    photos : []
  }),
  methods: {
    togglePictures(id){
      this.pictureAccess[id] = !this.pictureAccess[id];
      this.pictureAccess = JSON.parse(JSON.stringify(this.pictureAccess))
      if (this.pictureAccess[id])
        this.getPictures(id)
    },
    getPictures(id){
      fetch("https://json.medrating.org/photos?albumId=" + id)
          .then(response => response.json())
          .then(data => {
            this.photos[id] = data;
            let bookmarks = JSON.parse(localStorage.getItem("bookmarks"));
            const tempCondition = bookmarks && bookmarks.length > 0;
            for (let index in this.photos[id]){
              this.photos[id][index].inBookmarks = (tempCondition && bookmarks[this.photos[id][index].id])
            }
            this.photos = JSON.parse(JSON.stringify(this.photos));
          })
    },
    addToBookMarks(photoId, albumId){
      let bookmarks = []
      if (localStorage.getItem("bookmarks"))
        bookmarks = JSON.parse(localStorage.getItem("bookmarks"));
      const index = this.photos[albumId].findIndex( elem => elem.id == photoId)
      this.photos[albumId][index].inBookmarks = true;
      bookmarks[photoId] = this.photos[albumId][index];
      localStorage.setItem("bookmarks", JSON.stringify(bookmarks));
    },
    deleteFromBookMarks(photoId, albumId){
      let bookmarks = JSON.parse(localStorage.getItem("bookmarks"))
      bookmarks[photoId] = null;
      localStorage.setItem("bookmarks", JSON.stringify(bookmarks));
      const index = this.photos[albumId].findIndex( elem => elem.id == photoId)
      this.photos[albumId][index].inBookmarks = false;
    },
    updateBookmarks(element, value){
      const photoId = element.getAttribute('photo-id')
      const albumId = element.getAttribute('album-id')
      if (value){
        this.addToBookMarks(photoId, albumId);
        element.classList.add("star-active");
      }
      else{
        this.deleteFromBookMarks(photoId, albumId);
        element.classList.remove("star-active");
      }

    }
  },
  mounted() {
    for (let i in this.albums){
      this.pictureAccess[this.albums[i].id] = false;
    }
    const elements = document.querySelectorAll('#user-id-' + this.userid + ' .album-name');
    elements.forEach(element => {
      element.addEventListener("click", () => {
        element.children[0].classList.toggle("arrow-rotate");
      })
    })
  }
}
</script>

<style scoped>
  .album-name {
    margin-left: 25px;
    text-align: left;
    font-size: 13pt;
    cursor: pointer;
  }
  .arrow {
    display: inline-block;
  }
  .arrow-rotate {
    transform: rotate(90deg);
  }
</style>