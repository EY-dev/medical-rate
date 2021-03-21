<template>
 <div class="photo-gallery">
   <photo v-for="(photo, index) in photos[albumId]" :key="index" :photo="photo" :hideTitle="hideTitle" @update-bookmarks="updateBookmarks"/>
 </div>
</template>

<script>
import Photo from "@/components/Photo";
export default {
  name: "ListPhotos",
  components: {Photo},
  props: ['albumId', 'photos', 'hideTitle'],
  methods:{
    updateBookmarks(element, value){
      this.$emit("update-bookmarks", element, value);
    }
  },
}
</script>

<style scoped>
  .photo-gallery {
    width: 250px;
    margin-left: 100px;
  }
</style>