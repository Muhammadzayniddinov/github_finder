<script setup>
import axios from "axios";
import { ref } from "vue";

const data = ref([]);
const name = ref();

async function fetchData() {
  try {
    const res = await axios.get(`https://api.github.com/users/${name.value}`);
    data.value = res.data;
    console.log(data.value);
  } catch (error) {
    console.error(error);
  } finally {
    name.value = "";
  }
}
function formatDate(dateString) {
  const date = new Date(dateString);
  const day = date.getUTCDate().toString().padStart(2, "0");
  const month = (date.getUTCMonth() + 1).toString().padStart(2, "0"); // Months are 0-based in JavaScript
  const year = date.getUTCFullYear();
  const hours = date.getUTCHours().toString().padStart(2, "0");
  const minutes = date.getUTCMinutes().toString().padStart(2, "0");

  return `${day}-${month}-${year} ${hours}:${minutes} `;
}
</script>
<template>
  <div class="container">
    <div class="search_box">
      <input class="input" type="text" v-model="name" />
      <button class="search" @click="fetchData">Search</button>
    </div>
    <div class="box">
      <div class="image">
        <img class="img" :src="data.avatar_url" v-if="data.avatar_url" />
        <img class="img" src="../assets/images/picture_download.png" v-else />
        <a class="image_btn" :href="data.html_url">Visit</a>
      </div>
      <div class="about">
        <h3 class="name">{{ data.name || "Unknown" }}</h3>
        <p>Repositories:{{ data.public_repos }}</p>
        <p>
          Created at:
          <span v-show="data.created_at">{{
            formatDate(data.created_at)
          }}</span>
        </p>
        <p>Followers:{{ data.followers }}</p>
        <p>Followings:{{ data.following }}</p>
      </div>
    </div>
  </div>
</template>
<style scoped>
.container {
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
  padding: 0 15px;
}
.search_box {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.box {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
}
.image {
  width: 250px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.img {
  width: 200px;
  height: 200px;
  margin-bottom: 10px;
  border: 1px solid black;
}
.image_btn {
  background-color: black;
  width: 200px;
  height: 30px;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  text-decoration: none;
  cursor: pointer;
}
.about {
  width: 250px;
  margin-left: 30px;
}
.input {
  width: 250px;
  height: 30px;
}
.search {
  height: 34px;
  background: #000;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
