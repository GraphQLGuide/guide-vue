<template>
  <h2>Sections</h2>

  <div v-if="loading">Loading...</div>

  <div v-else-if="error">Error: {{ error.message }}</div>

  <div v-else-if="noSections">This chapter has no sections</div>

  <ul v-else>
    <li v-for="section of sections" :key="section.id">
      {{ section.number }}. {{ section.title }}
    </li>
  </ul>
</template>

<script>
import { useQuery, useResult } from '@vue/apollo-composable'
import { gql } from '@apollo/client/core'
import { computed } from 'vue'

export default {
  name: 'SectionList',
  props: {
    id: {
      type: Number,
      required: true
    }
  },
  setup(props) {
    const { result, loading, error } = useQuery(
      gql`
        query SectionList($id: Int!) {
          chapter(id: $id) {
            sections {
              id
              number
              title
            }
          }
        }
      `,
      props
    )

    const sections = useResult(result, [], data => data.chapter.sections)

    return {
      loading,
      error,
      noSections: computed(() => sections.value.length === 1),
      sections
    }
  }
}
</script>
