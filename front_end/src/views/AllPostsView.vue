<template>
    <div>
      <h1>All Posts</h1>
      <div v-if="loading">Loading...</div>
      <div v-if="error">{{ error.message }}</div>
      <ul v-if="result">
        <li v-for="post in result.allPosts" :key="post.slug">
          <h2>
            <router-link :to="{ name: 'post', params: { slug: post.slug } }">
              {{ post.title }}
            </router-link>
          </h2>
          <p>
            by 
            <router-link :to="{ name: 'author', params: { username: post.author.user.username } }">
              {{ post.author.user.username }}
            </router-link>
          </p>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { useQuery } from "@vue/apollo-composable";
  import gql from "graphql-tag";
  
  const ALL_POSTS_QUERY = gql`
    query {
      allPosts {
        title
        slug
        author {
          user {
            username
            firstName
            lastName
          }
        }
      }
    }
  `;
  
  const { result, loading, error } = useQuery(ALL_POSTS_QUERY);
  
  </script>
  
  <style scoped src="../style.css">
</style>