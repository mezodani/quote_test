<script setup>
import { ref } from 'vue'

const quote = ref(null)
const loading = ref(false)
const error = ref(null)

const API_URL = 'http://192.168.100.23:8000/api/quote'

const fetchQuote = async () => {
  loading.value = true
  error.value = null

  try {
    const response = await fetch(API_URL)
    if (!response.ok) throw new Error('Failed to fetch quote')
    quote.value = await response.json()
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
fetchQuote()
</script>

<template>
  <div class="app">
    <h1>Random Quote Generator</h1>

    <div class="quote-container" v-if="quote">
      <p class="quote">"{{ quote.quote }}"</p>
      <p class="artist">- {{ quote.artist }}</p>
    </div>

    <button @click="fetchQuote" :disabled="loading">
      {{ loading ? 'Loading...' : 'Get New Quote' }}
    </button>

    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>



<style scoped>
.app {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
  font-family: Arial, sans-serif;
}

.quote-container {
  background: #f5f5f5;
  padding: 30px;
  border-radius: 10px;
  margin: 20px 0;
}

.quote {
  font-size: 24px;
  font-style: italic;
  margin-bottom: 10px;
}

.artist {
  font-size: 18px;
  color: #666;
}

button {
  background: #4CAF50;
  color: white;
  border: none;
  padding: 12px 30px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover:not(:disabled) {
  background: #45a049;
}

button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.error {
  color: red;
  margin-top: 10px;
}
</style>