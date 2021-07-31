<template>
  <div id="app">
    <form @submit.prevent="findWords(inputText)">
      <input type="text" v-model="inputText" minlength="1">
      <button>Find Words</button>
      <span v-if="isFinding" class="spinner"><span>⏳</span></span>
    </form>
    <div v-if="isError">⚠️ Error fetching words list</div>
    <ul v-if="!isError && !isFinding">
      <li v-for="word in matchingWords" :key="word">{{ word }}</li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      isError: false,
      isFinding: false,
      isGettingWords: false,
      inputText: '',
      words: [],
      worker: null,
      matchingWords: []
    }
  },

  async mounted () {
    this.words = await this.getWords()
    this.worker = new Worker('worker.js');  
    this.worker.onmessage = (message => {
      this.isFinding = false
      this.matchingWords = this.words.filter(word => {
        return message.data.permutations.includes(word)
      })
    })
  },

  methods: {
    async findWords (string) {
      if (string.length > 0) {
        this.isFinding = true
        this.worker.postMessage({ string: string })
      }
    },

    async getWords () {
      this.isGettingWords = true
      this.isError = false
      try {
        const response = await axios.get('https://gist.githubusercontent.com/adamc00/a45beb8a0cb55593220f749838c534d0/raw/fd11dcce5df4098839e19f21c50cc2363b9b7863/words.txt')
        // Split the response into an array delimited by the newspace character
        return response.data.split('\n')
      } catch (e) {
        this.isError = true
        return []
      } finally {
        this.isGettingWords = false
      }
    }
  }
}
</script>

<style lang="scss">
.spinner {
  span {
    animation: spin 0.6s linear infinite;
    display: inline-block;
  }
}

@keyframes spin { 
  100% { transform:rotate(360deg); }
}
</style>
