<script setup lang="ts">
import { useQuery } from '@urql/vue'
import { graphql } from './gql'
import HelloWorld from "./components/HelloWorld.vue";

const AllFilmsQuery = graphql(/* GraphQL */ `
  query AllFilmsWithVariablesQuery($first: Int!) {
    allFilms(first: $first) {
      edges {
        node {
          id
        }
      }
    }
  }
`)

// These variables should raise a type error:
useQuery({
  query: AllFilmsQuery,
  variables: { first: null } // wrong type
})
useQuery({
  query: AllFilmsQuery,
  variables: {  } // missing field
})
useQuery({
  query: AllFilmsQuery,
  variables: { foo: 'bar' } // extra field
})

/* Now go to node_modules/@urql/vue/dist/urql-vue.d.ts and replace

type MaybeRefObj<T> = T extends {} ? {
    [K in keyof T]: MaybeRef<T[K]>;
} : T;

* with

type MaybeRefObj<T extends {}> = { [K in keyof T]: MaybeRef<T[K]> };

* Then the type of variables is correctly inferred! */
</script>

<template>
  <div>
    <a href="https://vite.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <HelloWorld msg="Vite + Vue" />
</template>
