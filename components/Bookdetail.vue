<template >
  <div class="grid grid-cols-6 gap-4">
    <div class="col-span-1 flex justify-center">
      <img
        v-if="bookDetail.volumeInfo.imageLinks"
        :src="bookDetail.volumeInfo.imageLinks.thumbnail"
        style="height:300px;width:200px"
      />
      <img
        v-else
        src="https://semantic-ui.com/images/wireframe/image.png"
        style="height:300px;width:200px"
      />
    </div>
    <div class="col-span-5 px-16">
      <div class="text-2xl">{{bookDetail.volumeInfo.title}}</div>
      <div
        v-if="bookDetail.volumeInfo.authors"
        class="text-md my-4"
      >{{bookDetail.volumeInfo.authors[0]}}</div>
      <div v-else>ไม่ได้ระบุ</div>
      <div class="text-xs">
        <p v-html="bookDetail.volumeInfo.description"></p>
      </div>
      <div>
           <t-button size="sm" disabled="true" class="bg-indigo-400 mr-4 my-5 text-white" @click="add">{{bookDetail.volumeInfo.pageCount}} pages</t-button>
          <t-button size="sm" class="bg-teal-400 mr-4 my-5 text-white" @click="add">Add to Favorite</t-button>
          <t-button size="sm" disabled="true" class="bg-indigo-600 mr-4 my-5 text-white">Preview</t-button>
      </div>
      
    </div>
    <t-modal ref="modal" height="20" class="text-center">Add to Favorite Success</t-modal>
  </div>
</template>

<script>
export default {
  props: {
    bookDetail: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      bookDetail: [],
      thumbnails: '',
      setBook: []
    }
  },
    created() {
    this.setBook = JSON.parse(localStorage.getItem('Storebook') || '[]')
  },
  methods: {
    add() {
      if (this.bookDetail.volumeInfo.imageLinks) {
        this.thumbnails = this.bookDetail.volumeInfo.imageLinks.thumbnail
      } else {
        this.thumbnails = 'https://semantic-ui.com/images/wireframe/image.png'
      }
      const data = {
        title: this.bookDetail.volumeInfo.title,
        thumbnail: this.thumbnails,
        id: this.bookDetail.id
      }
      this.setBook.push(data)
      localStorage.setItem('Storebook', JSON.stringify(this.setBook))
      this.$refs.modal.show()
    }
  }
}
</script>