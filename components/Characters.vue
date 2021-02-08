<template>
  <div class="container">
    <div v-for="(character, index) in results" :key="index">
      <a @click="$router.push('/'+character.id)">
        <div class="card">
          <div class="img-box">
            <img :src="character.image" class="img"/>
          </div>
          <div class="text">
            <h2>{{ character.name }}</h2>
          </div>
        </div>
      </a>
    </div>
    <el-divider/>
    <infinite-loading
      spinner="spiral"
      @infinite="infiniteScroll">
      <div slot="no-more">No more characters :(</div>
    </infinite-loading>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Characters",
  data() {
    return {
      results: [],
      page: 1
    };
  },
  computed: {
    url() {
      return "https://rickandmortyapi.com/api/character?page=" + this.page;
    }
  },
  created() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      const response = await axios.get(this.url);
      this.results = response.data.results;
    },
    infiniteScroll($state) {
      setTimeout(() => {
        this.page++
        axios.get(this.url)
          .then((response) => {
            if (response.data.results.length > 0) {
              response.data.results.forEach((item) => this.results.push(item))
              $state.loaded()
              if (!response.data.info.next) {
                $state.complete()
              }
            }
          }).catch((err) => {
          console.log(err)
        })
      }, 500)
    }
  }
}
</script>

<style scoped>
.card {
  width: 350px;
  height: 180px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.5rem;
  box-shadow: rgb(0, 0, 0) 0px 4px 6px -1px, rgb(0, 0, 0) 0px 2px 4px -1px;
}

.img-box {
  flex: 2 1 0;
}
.img {
  width: 100%;
  height: 100%;
  margin: 0px;
  opacity: 1;
  transition: opacity 0.5s ease 0s;
  object-position: center center;
  object-fit: cover;
}

.text {
  flex: 3 1 0;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
}

.container {
  display: flex;
  flex-wrap: wrap;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
}

</style>
