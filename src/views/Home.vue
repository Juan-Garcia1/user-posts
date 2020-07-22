<template>
  <div class="home">
    <p v-if="error">{{ errorMsg }}</p>
    <div v-else>
      <p v-if="loading">Loading...</p>

      <ul class="user-list">
        <li v-for="user in users" :key="user.id" class="user">
          <router-link
            :to="{ name: 'UserPosts', params: { userId: user.id } }"
            >{{ user.name }}</router-link
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  name: "Home",
  components: {},
  data: function() {
    return {
      users: [],
      loading: true,
      error: false,
      errorMsg: "",
    };
  },
  mounted: function() {
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((res) => {
        this.users = res.data;
      })
      .catch((err) => {
        this.error = true;
        this.errorMsg = err;
      })
      .finally(() => {
        this.loading = false;
      });
  },
};
</script>
