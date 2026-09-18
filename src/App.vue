<script setup lang="ts">
import { onMounted, ref } from 'vue'

const apiBaseUrl = import.meta.env.VITE_API_BASE_URL as string
const status = ref<'loading' | 'ok' | 'error'>('loading')
const message = ref('')

onMounted(async () => {
  try {
    const res = await fetch(`${apiBaseUrl}/health`)
    const data = await res.json()
    status.value = res.ok ? 'ok' : 'error'
    message.value = JSON.stringify(data)
  } catch (err) {
    status.value = 'error'
    message.value = err instanceof Error ? err.message : String(err)
  }
})
</script>

<template>
  <main>
    <h1>cuanly</h1>
    <p>API base URL: <code>{{ apiBaseUrl }}</code></p>
    <p>Backend health check: <strong>{{ status }}</strong> — {{ message }}</p>
  </main>
</template>
