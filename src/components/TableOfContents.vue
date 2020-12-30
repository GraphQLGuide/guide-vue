<template>
  <div v-if="loading">Loading...</div>

  <div v-else-if="error">Error: {{ error.message }}</div>

  <ul>
    <li v-for="chapter of chapters" :key="chapter.id">
      {{
        chapter.number ? `${chapter.number}. ${chapter.title}` : chapter.title
      }}
    </li>
  </ul>
</template>

<script>
import { gql } from '@apollo/client/core'
import { useQuery, useResult } from '@vue/apollo-composable'

export default {
  name: 'TableOfContents',
  setup() {
    const { result, loading, error } = useQuery(gql`
      query ChapterList {
        chapters {
          id
          number
          title
        }
      }
    `)

    const chapters = useResult(result, [])

    return {
      loading,
      error,
      chapters
    }
  }
}
</script>
