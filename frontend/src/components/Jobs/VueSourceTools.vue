<script setup>
import { onMounted, watch, ref } from 'vue'
import { useJobsStore } from '@/store/jobs'
import sourceTools from '@/data/jobs-source-tools.json'

const checked = ref([])
const jobStore = useJobsStore()

onMounted(() => {
  const sourceFieldsFromStorage = JSON.parse(localStorage.getItem('sources'))
  if (sourceFieldsFromStorage?.length) {
    checked.value = sourceFieldsFromStorage
  }
})

watch(checked, () => {
  localStorage.setItem('sources', JSON.stringify(checked.value))
  jobStore.changeQuery('sources', checked.value)
  jobStore.fetchJobsData()
  jobStore.page = 1
})
</script>

<template>
  <v-checkbox
    v-for="tool in sourceTools"
    :key="tool.value"
    v-model="checked"
    :label="tool.label"
    :value="tool.value"
    density="compact"
    hide-details
    disabled
  ></v-checkbox>
</template>
