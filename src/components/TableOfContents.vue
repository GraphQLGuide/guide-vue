<template>
  <div class="TableOfContents">
    <li v-for="chapter of chapters" :key="chapter.id">
      {{ chapter.title }}
    </li>
  </div>
</template>

<script>
import { useQuery, useResult } from '@vue/apollo-composable'
import { gql } from '@apollo/client/core'

export default {
  name: 'TableOfContents',
  setup() {
    const { result, loading, error } = useQuery(gql`
      query ChapterQuery {
        chapters {
          id
          number
          title
          sections {
            id
            number
            title
          }
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

<style scoped></style>
