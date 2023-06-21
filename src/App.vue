<template>
  <div>
    <div id="nav">
      <router-link to="/">Главная</router-link>
      <router-link to="/todos">ToDo-List</router-link>
    </div>
    <router-view />
  </div>
</template>

<script>
import { useStore } from 'vuex'
export default {
  beforeCreate() {
    const store = useStore()
    store.commit('loadStore');
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  display: flex;
  justify-content: center;
  padding: 2rem;
  width: 100%;
  height: 100vh;
  background: linear-gradient(90deg, #b9deed, #efefef);
}

h1 {
  font-weight: 900;
  font-size: 2rem;
  margin: 3rem 0 2rem 0;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
  margin: 0 1rem 0 0;
  background: #eee;
  border-radius: 14px;
  text-decoration: none;
  padding: 0.25rem 0.75rem;
}

#nav a.router-link-exact-active {
  color: #0428ff;
}

input[type="submit"] {
  background: #0028ff;
  color: white;
  border-radius: 10px;
  padding: 0.5rem 1rem;
  border: none;
  box-shadow: 0 5px 10px #0428ff7a;
  font-size: 1rem;
  font-weight: 600;
  transition: all 0.25s ease-out;
  cursor: pointer;
}

input[type="checkbox"] {
  width: auto;
  margin: 0 0.5rem 0 0;
}

input[type="submit"]:hover {
  background: #3600ff;
  box-shadow: 0 3px 7px #0428ff7a;
}

input {
  border-radius: 10px;
  box-sizing: border-box;
  border: 1px solid #aaa;
  font-size: 1rem;
  width: 100%;
  padding: 0.5rem 1rem;
}
</style>
