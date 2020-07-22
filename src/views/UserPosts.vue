<template>
  <div>
    <p v-if="error"
      >{{ errorMsg }} <router-link to="/">Go to homepage</router-link>
    </p>
    <div v-else>
      <p v-if="loading">Loading...</p>
      <ul v-else>
        <li v-for="post in userPosts" :key="post.id" class="post">
          <h2 class="post__title">{{ post.title }}</h2>
          <p class="post__author">post by: {{ user.name }}</p>
          <p class="post__excerpt">{{ post.body }}</p>
          <router-link :to="{ name: 'SinglePost', params: { postId: post.id } }"
            >Read more</router-link
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "UserPosts",
  data: function() {
    return {
      loading: true,
      error: false,
      errorMsg: "",
      user: {},
      userPosts: [],
    };
  },
  mounted: function() {
    const userId = this.$route.params.userId;

    const fetchUser = axios.get(
      `https://jsonplaceholder.typicode.com/users/${userId}`
    );
    const fetchUserPosts = axios.get(
      `https://jsonplaceholder.typicode.com/users/${userId}/posts`
    );

    Promise.all([fetchUser, fetchUserPosts])
      .then(([user, posts]) => {
        this.user = user.data;
        this.userPosts = posts.data;
      })
      .catch(() => {
        this.error = true;
        this.errorMsg = "Oops... we were unable to fetch this user's posts. ";
      })
      .finally(() => {
        this.loading = false;
      });
  },
};
</script>

<style scoped>
.post__excerpt {
  overflow: hidden;
  white-space: nowrap;
  max-width: 80%;
  text-overflow: ellipsis;
}
</style>
