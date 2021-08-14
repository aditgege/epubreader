<template>
  <div class="w-full flex  items-center justify-center pt-3">
    <div class="flex items-center h-screen justify-center flex-col w-4/12">
      pilih buku pin, ini format epub yaah
      <button v-for="book, i in books" :key="i" :to="book.url" class="bg-black text-white my-3 px-4 py-2 shadow-md " @click="openBook(book.url)">
        {{ book.name }}
      </button>
    </div>
  </div>
</template>

<script>
// import PDF from 'pdfvuer'
export default {
  data () {
    return {
      books: [
        { name: 'Khazanah', url: '/reader/index.html' },
        { name: 'Moby Dick', url: '/reader/index.html?bookPath=https://s3.amazonaws.com/moby-dick/' },
        { name: 'Alice', url: '/reader/index.html?bookPath=https://s3.amazonaws.com/epubjs/books/alice.epub' }
      ]
    }
  },
  methods: {
    openBook (url) {
      // this.$router.push(url)
      window.open(url, '_blank')
    }
  }
}
</script>
