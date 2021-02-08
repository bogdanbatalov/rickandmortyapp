<template>
  <div class="container">
    <div class="card" v-if="data">
      <div class="img-box">
        <img class="img" :src="data.image" />
      </div>
      <div class="text">
        <p>Name: {{ data.name }}</p>
        <p>Status: {{ data.status }}</p>
        <p>Gender: {{ data.gender }}</p>
        <p>Location: {{ data.location.name }}</p>
      </div>
    </div>
    <div v-else>
      Character not found!
    </div>
    <el-divider />
    <el-button @click="$router.push('/')">Back to characters list</el-button>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: "CharacterInfo",
  props: {
    id: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      data: {
        id: null,
        name: null,
        status: null,
        gender: null,
        location: {
          name: null
        },
        image: null,
      }
    };
  },
  computed: {
    url() {
      return "https://rickandmortyapi.com/api/character/" + this.id
    }
  },
  created() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      axios.get(this.url)
        .then((response) => {
          this.data = response.data
        }).catch((err) => {
        this.data = null
        console.log(err)
      })
    }
  }
}
</script>

<style scoped>

.card {
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.5rem;
  box-shadow: rgb(0, 0, 0) 0px 4px 6px -1px, rgb(0, 0, 0) 0px 2px 4px -1px;
}

.text {
  flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
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

.container {
  display: flex;
  flex-wrap: wrap;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
  align-items: center;
}
</style>
