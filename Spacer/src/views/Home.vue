<template>
  <div class="wrapper">
    <div class="search">
      <label>Search</label>
      <input
        id="search"
        name="search"
        v-model="searchValue"
        @keyup="handleInput">
    </div>
    <ul>
      <li v-for="result in apiResults" :key="result.data[0].nasa_id">
        <p>{{result.data[0].title}}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const apiUrl = 'https://images-api.nasa.gov/search';

export default {
  name: 'Home',
  data() {
    return {
      searchValue: '',
      apiResults: [],
    };
  },
  methods: {
    handleInput: debounce(function () {
      axios.get(`${apiUrl}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.apiResults = response.data.collection.items;
        });
    }, 500),
  },
};
</script>

<style lang="scss" scoped>
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 30px;
    width: 100%;
  }

  .search {
    display: flex;
    flex-direction: column;
    width: 250px;
  }

  label {
    font-family: "JetBrains Mono", sans-serif;
  }

  input {
    height: 30px;
    border: 0;
    border-bottom: 1px solid black;
  }
</style>
