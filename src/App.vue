<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <h1>The GraphQL Guide</h1>
  <TableOfContents />
</template>

<script>
import { ApolloClient, InMemoryCache } from '@apollo/client/core'
import { DefaultApolloClient } from '@vue/apollo-composable'
import { provide } from 'vue'

import TableOfContents from './components/TableOfContents.vue'

const client = new ApolloClient({
  uri: 'https://api.graphql.guide/graphql',
  cache: new InMemoryCache()
})

export default {
  name: 'App',
  components: {
    TableOfContents
  },
  setup() {
    provide(DefaultApolloClient, client)
  }
}
</script>
