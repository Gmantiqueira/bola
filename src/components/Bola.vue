<template>
  <v-card class="mx-auto pb-1" max-width="344">
    <v-img :src="repository + imgLink" height="auto"></v-img>

    <v-card-title class="d-flex justify-center">{{ title }}</v-card-title>

    <div class="mb-5">
      <v-btn
        @click.prevent="downloadItem(repository + imgLink, title)"
        x-large
        color="red"
        dark
        >Baixar a bola</v-btn
      >
    </div>
  </v-card>
</template>

<script>
import Axios from 'axios'

export default {
  props: ['imgLink', 'title'],
  data: () => ({
    error: ''
  }),
  computed: {
    repository() {
      return process.env.NODE_ENV == 'development' ? '' : '/bola'
    }
  },
  methods: {
    downloadItem(url, title) {
      /* eslint-disable no-console */
      Axios.get(url, { responseType: 'blob' })
        .then(({ data }) => {
          const blob = new Blob([data], { type: 'application/jpg' })
          let link = document.createElement('a')
          link.href = window.URL.createObjectURL(blob)
          link.download = title + '.jpg'
          link.click()
        })
        .catch((error) => {
          this.error = error
        })
    }
  }
}
</script>
