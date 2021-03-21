<template>
  <div id="app">
    <nav class="nav-bar">
      <ul class="nav-list">
        <li class="nav-item"><a href="#" @click="isCatalogue = true">Каталог</a></li>
        <li class="nav-item"><a href="#" @click="isCatalogue = false">Избранное</a></li>
      </ul>
    </nav>
    <div class="main">
      <catalogue v-if="isCatalogue" :users="users"/>
      <bookmark v-else/>
    </div>
  </div>
</template>

<script>

const Catalogue = () => import('./components/Catalogue');
const Bookmark = () => import('./components/Bookmark');

export default {
  name: 'App',
  components: {
    Bookmark,
    Catalogue
  },
  data: () => ({
    isCatalogue : true,
    users : null
  }),
  created() {
    fetch("https://json.medrating.org/users")
        .then(response => response.json())
        .then(data => this.users = data.filter((user) => (user.name)))
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 800px;
  min-width: 300px;
  width: 100%;
  position: absolute;
  top:0;
  left: 50%;
  transform: translate(-50%, 0);
}
.nav-bar {
  width: 100%;
  height: 50px;
  font-size: 20pt;
  margin-bottom: 50px;
}
.nav-list {
  display: flex;
  list-style: none;
  padding: 0;
  justify-content: space-between;
  align-items: center;
}
.nav-item {
  flex: 1;
  width: 100%;
}
.main {
  height: calc(100% - 100px);
}
</style>
