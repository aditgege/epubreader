<template>
  <div class="home_wrap">
    <div class="pdf_down">
      <div class="pdf_set_left" @click="scaleD()">
        Zoom IN
      </div>
      <div class="pdf_set_middle" @click="scaleX()">
        Zoom Out
      </div>
    </div>

    <div :style="{width:pdf_div_width,margin:'0 auto'}">
      <canvas v-for="page in pdf_pages" :id="'the_canvas'+page" :key="page" />
    </div>
  </div>
</template>

<script>
const PDFJS = require('pdfjs-dist')
PDFJS.GlobalWorkerOptions.workerSrc = require('pdfjs-dist/build/pdf.worker.entry.js')
export default {
  data () {
    return {
      pdf_scale: 1.0,
      pdf_pages: [],
      pdf_div_width: '',
      pdf_src: null
    }
  },
  mounted () {
    this.get_pdfurl()
  },
  methods: {
    scaleD () {
      let max = 0
      if (window.screen.width > 1440) {
        max = 1.4
      } else {
        max = 1.2
      }
      if (this.pdf_scale >= max) {
        return
      }
      this.pdf_scale = this.pdf_scale + 0.1
      this._loadFile(this.pdf_src)
    },
    scaleX () {
      const min = 1.0
      if (this.pdf_scale <= min) {
        return
      }
      this.pdf_scale = this.pdf_scale - 0.1
      this._loadFile(this.pdf_src)
    },
    get_pdfurl () {
      this.pdf_src = 'https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf'
      this._loadFile(this.pdf_src)

      // this.$axios.get('')
      //   .then((res) => {
      //     this.pdf_src = res.url
      //     this._loadFile(this.pdf_src)
      //   })
    },
    _loadFile (url) {
      const loadingTask = PDFJS.getDocument(url)
      loadingTask.promise
        .then((pdf) => {
          this.pdfDoc = pdf
          this.pdf_pages = this.pdfDoc.numPages
          // debugger
          this.$nextTick(() => {
            this._renderPage(1)
          })
        })
    },
    _renderPage (num) {
      const that = this
      this.pdfDoc.getPage(num)
        .then((page) => {
          const canvas = document.getElementById('the_canvas' + num)
          const ctx = canvas.getContext('2d')
          const dpr = window.devicePixelRatio || 1
          const bsr = ctx.webkitBackingStorePixelRatio ||
              ctx.mozBackingStorePixelRatio ||
              ctx.msBackingStorePixelRatio ||
              ctx.oBackingStorePixelRatio ||
              ctx.backingStorePixelRatio || 1
          const ratio = dpr / bsr
          const viewport = page.getViewport({ scale: this.pdf_scale })
          canvas.width = viewport.width * ratio
          canvas.height = viewport.height * ratio
          canvas.style.width = viewport.width + 'px'
          that.pdf_div_width = viewport.width + 'px'
          canvas.style.height = viewport.height + 'px'
          ctx.setTransform(ratio, 0, 0, ratio, 0, 0)
          const renderContext = {
            canvasContext: ctx,
            viewport
          }
          page.render(renderContext)
          if (this.pdf_pages > num) {
            this._renderPage(num + 1)
          }
        })
    }
  }
}
</script>
<style lang="scss" >
#viewer {
  margin-left:20px;
}
div.page {
  float:left;
}
.home_wrap{
   width: 100%;
   height: 100%;
   .pdf_down{
    position:fixed;
    display: flex;
    z-index: 20;
    right:26px;
    bottom:7%;
    .pdf_set_left{
         width: 30px;
         height: 40px;
         color: #408FFF;
         font-size: 11px;
         padding-top:25px;
         text-align: center;
         margin-right: 5px;
         cursor: pointer;
    }
    .pdf_set_middle{
         width: 30px;
         height: 40px;
         color: #408FFF;
         font-size: 11px;
         padding-top:25px;
         text-align: center;
         margin-right: 5px;
         cursor: pointer;
    }
  }
}
</style>
