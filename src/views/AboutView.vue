<template>
  <div>
  <div v-for="abouts in about" :key="abouts.id" class="about">
    <router-link :to="{name: 'AboutDetails', params: { id: abouts.id }}">
      <h2>{{ abouts.tittle }}</h2>
    </router-link>
  </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      about: [
        { tittle: 'Mahasiswa UIR', id: 1, details: 'lorem' },
        { tittle: 'Contact Me', id: 2, details: 'lorem' },
        
      ]
    }
  }
}
</script>

<style>
.about h2{
  background: #c0cc52;
  font-size: small;
  padding: 20px;
  border-radius: 10px;
  margin: 10px auto;
  max-width: 600px;
  cursor: pointer;
  color: #444;
}
.about h2:hover {
  background: #ddd;
}
.about a{
  text-decoration: none;
}
</style>