<template>
  <div class="user" :id="'id' + user.id" :user-id="user.id">
    <div class="name" @click="hideAlbums = !hideAlbums">
      <span class="arrow">></span> {{user.name}}
    </div>
    <albums v-if="!hideAlbums && albums" :userid="user.id" :albums="albums"/>
  </div>
</template>

<script>
import Albums from "@/components/Albums";
export default {
  name: "UserInfo",
  components: {Albums},
  props: ['user'],
  data: ()=> ({
    hideAlbums : true,
    albums : null
  }),
  methods: {
    updateAlbums(){
      fetch("https://json.medrating.org/albums?userId=" + this.user.id)
          .then(response => response.json())
          .then(data => this.albums = data)
    }
  },
  mounted() {
    const element = document.querySelector('#id' + this.user.id + ' .name');
    element.addEventListener("click", () => {
      if (!this.albums){
        this.updateAlbums();
      }
      element.children[0].classList.toggle("arrow-rotate");
    })
  }
}
</script>

<style scoped>
  .user {
    width: 100%;
    margin-bottom: 5px;
  }
  .name {
    text-align: left;
    font-size: 15pt;
    cursor: pointer;
  }
  .arrow {
    display: inline-block;
  }
  .arrow-rotate {
    transform: rotate(90deg);
  }

</style>