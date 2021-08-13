<template>
  <div class="w-full flex  items-center justify-center pt-3">
    <div class="flex items-center justify-center flex-col w-4/12">
      <div>
        <!-- {{ currentPage }} / {{ pageCount }} -->
      </div>
      <!-- <Fwturn ref="test" :options="options"> -->
      <Pdf
        class="border border-2 border-black"
        src="/cerita.pdf"
        :page="displayPage"
        @num-pages="pageCount = $event"
        @page-loaded="currentPage = $event"
      />
    </div>
  </div>
</template>

<script>
// import PDF from 'pdfvuer'
export default {
  data () {
    return {
      currentPage: 0,
      pageCount: 0,
      displayPage: 1,
      options: {
        display: 'single',
        acceleration: true,
        elevation: 50
      }
    }
  },
  methods: {
    testyu () {
      this.displayPage++
      this.$refs.test.next()
    }
  }
}
</script>
