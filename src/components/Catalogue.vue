<template>
  <div class="user-list">
    <user-info v-for="(value, index) in users" :key="index" :user="value"/>
  </div>
</template>

<script>
import UserInfo from "@/components/UserInfo";
export default {
  name: "Catalogue",
  props:['users'],
  components: {UserInfo},
}
</script>

<style scoped>
    .user-list {
      float: left;
    }
</style>