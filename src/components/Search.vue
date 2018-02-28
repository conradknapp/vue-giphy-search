<template>
  <div class="root">
    <form @submit="fetchData">
  <input v-model.lazy="query" @keydown="checkKey" />
  <select v-model="selectedLimit">
    <option v-for="({ text, value }) in optionsLimit" :key="value" :value="value">
      {{ text }}
    </option>
    </select>
  <select v-model="selectedType">
    <option v-for="({ text, value }) in optionsType" :key="value" :value="value">
      {{ text }}
    </option>
    </select>
    <button @click="fetchData">Submit</button> 
 </form>
  <main>
   <img :key="i" v-for="(result, i) in results" :style="{ 'background-image': 'url(' + result.images.original.url + ')' }" />
  </main>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Search',
  data() {
    return {
      base_url: `https://api.giphy.com/v1/gifs/`,
      selectedType: 'search',
      optionsType: [
      { text: 'search', value: 'search' },
      { text: 'trending', value: 'trending' },
      // { text: 'random', value: 'random' }
    ],
      query: 'steve brule check it out', 
      key:  'dc6zaTOxFJmzC',
      selectedLimit: '5',
      optionsLimit: [
        { text: '5', value: 5 },
        { text: '10', value: 10 },
        { text: '25', value: 25 },
        { text: '100', value: 100 }
      ],
      results: [],
      newSearch: false
    } 
  }, 
  beforeMount() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      if (event) {
     event.preventDefault();
      }
      if (this.newSearch) this.results = ''; axios.get(`${this.base_url}${this.selectedType}?q=${this.query}&apikey=${this.key}&limit=${this.selectedLimit}`)
      .then(({ data: { data } }) => {
       this.results = data;
   });  
    },
    checkKey(e) {
      if (e.keyCode === 13) {
        this.newSearch = false;
      } else {
        this.newSearch = true;
      }
    }
  }
}
</script>

<style>
body {
  background: #000;
}

form {
  text-align: center;
  padding: 2em;
}

input, button, select{
  font-size: 1.5rem;
  margin: 0.3em;
  background: #000;
  color:lightcoral;
}

img {
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
  background-size: cover;
  background-color: gray;
}

img:nth-of-type(n + 1) {
  background-color: purple;
}

img:nth-of-type(2n) {
  background-color: lightgoldenrodyellow;
}

img:nth-of-type(3n + 1) {
  background-color: mistyrose;
}

main {
  margin: 0 auto;
  max-width: 1000px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  grid-gap: 1em;
  grid-auto-rows: minmax(250px, auto);
}
</style>
