<template>
  <h1>The GraphQL Guide</h1>

  <div v-if="loading">Loading...</div>

  <div v-else-if="error">Error: {{ error.message }}</div>

  <ul>
    <li v-for="chapter of chapters" :key="chapter.id">
      <a @click="updateCurrentSection(chapter.id)">
        {{ (chapter.number ? chapter.number + '. ' : '') + chapter.title }}
      </a>
    </li>
  </ul>

  <SectionList v-bind:id="currentSection" />
</template>

<script>
import { useQuery, useResult } from '@vue/apollo-composable'
import { gql } from '@apollo/client/core'
import { ref } from 'vue'

import SectionList from './SectionList.vue'

const PREFACE_ID = -2

export default {
  name: 'TableOfContents',
  components: {
    SectionList
  },
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

    const currentSection = ref(PREFACE_ID)

    return {
      loading,
      error,
      chapters,
      currentSection,
      updateCurrentSection: newSection => (currentSection.value = newSection)
    }
  }
}
</script>
