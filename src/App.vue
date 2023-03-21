<template>
  <div>
    <header>
      <div class="logo" @mouseover="hover = true" @mouseleave="hover = false">
        <img src="./assets/logo.png" alt="GitHub Logo">
        <span :class="{ 'bold': hover }">GitHub Profiles</span>
      </div>
      <button v-if="searched" @click="searched = false">Change Username</button>
    </header>

    <main>
      <div class="search-container" v-if="!searched">
        <input type="text" v-model="username" placeholder="Insert a username" @keyup.enter="searchUser">
      </div>

      <div class="result" v-if="searched">
        <div v-if="user">
          <div class="profile-container">
            <img :src="user.avatar_url" alt="Avatar do usuário" class="avatar">
            <h2 class="name">{{ user.login }}</h2>
          </div>
            <h3 @click="$event => listShow = 1">Repositórios {{ repos.length }}</h3>
            <h3 @click="$event => listShow = 2">Starred {{ starred.length }}</h3>
          <div>

          </div>

          <div class="list" v-if="listShow == 1">
            <ul class="repos">
              <li v-for="repo in repos" :key="repo.id">
                <div>
                  <a :href="repo.html_url">{{ repo.name }}</a>
                  <p>{{ repo.language }} {{ repo.forks }}</p>
                </div>
              </li>
            </ul>
          </div>

          <div class="list" v-if="listShow == 2">
            <ul class="repo">
              <li v-for="st in starred" :key="st.id">
                <a :href="st.html_url">{{ st.name }}</a> - {{ st.language }} ({{ st.forks }} branches)
              </li>
            </ul>
          </div>
        </div>
  
        <div v-else>
          <p>Nenhum usuário pesquisado ainda</p>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      username: '',
      user: null,
      repos: [],
      starred: [],
      searched: false,
      hover: false,
      listShow: 1,
    }
  },

  methods: {
    async searchUser() {
      try {
        const { data: user } = await axios.get(`https://api.github.com/users/${this.username}`)
        this.user = user
        const { data: repos } = await axios.get(user.repos_url)
        this.repos = repos
        const { data: starred } = await axios.get(`https://api.github.com/users/${this.username}/starred`)
        this.starred = starred
        this.searched = true
      } catch (err) {
        console.error(err)
      }
    }
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  list-style-type: none;
  margin: 0;
  padding: 0;
  text-decoration: none;
  font-family: "Open Sans",Arial,Helvetica;
  font-weight: 400;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #24292e;
  color: white;
}

.result {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.logo {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.logo img {
  width: 50px;
  margin-right: 10px;
}

header h1 {
  font-size: 24px;
  font-weight: bold;
}

.bold {
  font-weight: bold;
}

.search-container {
  display: flex;
  justify-content: center;
  margin-top: 50px;
}

.search-container input {
  width: 300px;
  padding: 8px 30px 8px 10px;
  border: solid 1px;
}

.fa-search {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  font-size: 18px;
  color: #7d7d7d;
  cursor: pointer;
}

.search-container input:focus + .fa-search {
  color: #24292e;
}

.search-box button {
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 10px;
  cursor: pointer;
}

.avatar {
  width: 150px;
  border-radius: 200px;
}

.user-container {
  display: flex;
  align-items: center;
  margin-top: 50px;
}

.user-info {
  text-align: center;
  margin-right: 50px;
}

.user-info h2 {
  margin-bottom: 10px;
}

.user-repos {
  margin-left: 50px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  margin-bottom: 10px;
  border-bottom: solid 1px;
  display: flex;
  justify-content: flex-start;
}

button {
  background-color: #24292e;
  color: #eef0f1;
  border: solid 3px #eef0f1;
  border-radius: 20px;
  font-size: 18px;
  font-weight: bold;
  padding: 5px;
}
</style>
