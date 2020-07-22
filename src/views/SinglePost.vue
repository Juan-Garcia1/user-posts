<template>
  <div>
    <p v-if="loading">Loading...</p>
    <div v-else>
      <h1>{{ post.title }}</h1>
      <p>{{ post.body }}</p>
      <p
        >like to read more from this user?
        <router-link
          :to="{ name: 'UserPosts', params: { userId: post.userId } }"
          >click here</router-link
        >
        to view more.</p
      >
    </div>
    <h2>Comments</h2>
    <ul>
      <li v-for="comment in comments" :key="comment.id" class="comment">
        <h4 class="comment__name">{{ comment.name }}</h4>
        <span class="comment__email">{{ comment.email }}</span>
        <p class="comment__body">{{ comment.body }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "SinglePost",
  data: function() {
    return {
      loading: true,
      post: {},
      comments: [],
    };
  },
  mounted: function() {
    const postId = this.$route.params.postId;

    const getSinglePost = axios.get(
      `https://jsonplaceholder.typicode.com/posts/${postId}`
    );

    const getPostComments = axios.get(
      `https://jsonplaceholder.typicode.com/posts/${postId}/comments`
    );

    Promise.all([getSinglePost, getPostComments])
      .then(([post, comments]) => {
        this.post = post.data;
        this.comments = comments.data;
      })
      .finally(() => (this.loading = false));
  },
};
</script>

<style></style>
