<template>
    <div>
      <div v-if="loading">Loading...</div>
      <div v-if="error">{{ error.message }}</div>
      <div v-if="result">
        <h1>{{ result.authorByUsername.user.firstName }} {{ result.authorByUsername.user.lastName }}</h1>
        <p>Username: {{ result.authorByUsername.user.username }}</p>
        <p>Bio: {{ result.authorByUsername.bio }}</p>
        <p>Website: <a :href="result.authorByUsername.website" target="_blank">{{ result.authorByUsername.website }}</a></p>
        <h2>Posts</h2>
        <ul>
          <li v-for="post in result.authorByUsername.postSet" :key="post.slug">
            <router-link :to="{ name: 'post', params: { slug: post.slug } }">
              {{ post.title }}
            </router-link>
            <ul>
              <li v-for="tag in post.tags" :key="tag.name">
                <router-link :to="{ name: 'tag', params: { tag: tag.name } }">
                  {{ tag.name }}
                </router-link>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script setup>
  import { useRoute } from "vue-router";
  import { useQuery } from "@vue/apollo-composable";
  import gql from "graphql-tag";
  
  const route = useRoute();
  const username = route.params.username;
  
  const AUTHOR_QUERY = gql`
    query($username: String!) {
      authorByUsername(username: $username) {
        website
        bio
        user {
          firstName
          lastName
          username
        }
        postSet {
          title
          slug
          tags {
            name
          }
        }
      }
    }
  `;
  
  const { result, loading, error } = useQuery(AUTHOR_QUERY, { username });
  </script>
  
  
  
  
  
  