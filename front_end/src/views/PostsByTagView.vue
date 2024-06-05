<template>
  <div>
    <h1>Posts tagged with "{{ tag }}"</h1>
    <div v-if="loading">Loading...</div>
    <div v-if="error">{{ error.message }}</div>
    <ul v-if="result">
      <li v-for="post in result.postsByTag" :key="post.slug">
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
import { useRoute } from "vue-router";
import { useQuery } from "@vue/apollo-composable";
import gql from "graphql-tag";

const route = useRoute();
const tag = route.params.tag;

const POSTS_BY_TAG_QUERY = gql`
  query($tag: String!) {
    postsByTag(tag: $tag) {
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

const { result, loading, error } = useQuery(POSTS_BY_TAG_QUERY, { tag });
</script>
