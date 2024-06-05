<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-if="error">{{ error.message }}</div>
    <div v-if="result">
      <h1>{{ result.postBySlug.title }}</h1>
      <h2>{{ result.postBySlug.subtitle }}</h2>
      <p>Published on {{ displayableDate(result.postBySlug.publishDate) }}</p>
      <p>
        by 
        <router-link :to="{ name: 'author', params: { username: result.postBySlug.author.user.username } }">
          {{ result.postBySlug.author.user.username }}
        </router-link>
      </p>
      <div v-html="result.postBySlug.body"></div>
      <div>
        <h3>Tags</h3>
        <ul>
          <li v-for="tag in result.postBySlug.tags" :key="tag.name">{{ tag.name }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useRoute } from "vue-router";
import { useQuery } from "@vue/apollo-composable";
import gql from "graphql-tag";

const route = useRoute();
const slug = route.params.slug;

const POST_QUERY = gql`
  query($slug: String!) {
    postBySlug(slug: $slug) {
      title
      subtitle
      publishDate
      published
      metaDescription
      slug
      body
      author {
        user {
          username
          firstName
          lastName
        }
      }
      tags {
        name
      }
    }
  }
`;

const { result, loading, error } = useQuery(POST_QUERY, { slug });

const dateFormatter = new Intl.DateTimeFormat("en-US", { dateStyle: "full" });
const displayableDate = (date) => dateFormatter.format(new Date(date));
</script>

