<template>
    <div>
    <header>
      <div class="logo">
        <img src="./assets/logo.png" alt="GitHub Logo" @mouseover="hover = true" @mouseleave="hover = false">
        <span :class="{ 'bold': hover }">GitHub Profiles</span>
      </div>
    </header>

    <div class="search-box">
      <input type="text" placeholder="Digite o username do Github" v-model="username" @keyup.enter="searchProjects">
    </div>

    <div v-if="user">
      <img :src="user.avatar_url" alt="Avatar" id="avatar">
      <h2>{{ user.login }}</h2>
    </div>
    
    <ul v-if="projects.length > 0">
      <li v-for="project in projects" :key="project.id">
        <a :href="project.html_url">{{ project.name }}</a>
      </li>
    </ul>
    <div v-if="projects.length === 0 && searched">
      <p>Nenhum projeto encontrado</p>
    </div>
  </div>
</template>
<script>


export default {
  data() {
    return {
      username: "",
      user: null,
      projects: [],
      searched: false
    };
  },
  methods: {
    async searchProjects() {
      this.searched = true;
      const response = await fetch(`https://api.github.com/users/${this.username}/repos`);
      if (response.status === 404) {
        this.projects = [];
        this.user = null;
        return;
      }
      const projects = await response.json();
      this.projects = projects;

      const userResponse = await fetch(`https://api.github.com/users/${this.username}`);
      const user = await userResponse.json();
      this.user = user;
    }
  }
};
</script>

<style>
*{
  box-sizing: border-box;
    list-style-type: none;
    margin: 0;
    padding: 0;
    text-decoration: none;
    font-family: "Open Sans",Arial,Helvetica;
    /* font-size: 7px; */
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
  border: none;
  border-radius: 20px;
  background-color: #eef0f1;
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
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  margin-bottom: 10px;
}
</style>
