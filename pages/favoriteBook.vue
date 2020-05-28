<template>
  <div>
     <div class="flex justify-start">
    <div>
      <nuxt-link to="/">
        <t-button size="sm" class="ml-4 my-5 text-black">Back</t-button>
      </nuxt-link>
    </div>
  </div>
      <div class="grid grid-cols-5 gap-3">
        <div v-for="items in favoriteBook" :key="items.id" class="flex justify-center flex-col mt-2">
            <t-card
          class="col-3 mx-12"
              style="width:200px;height:250px; "
          @click.native="bookdetail(items.id)"
        >
          <img
            v-if="items.thumbnail"
            :src="items.thumbnail"
            style="height:180px;width:150px"
          />
          <img v-else src="https://semantic-ui.com/images/wireframe/image.png" style="height:180px;width:150px" />
          <div class="cut-text text-xs text-center mt-2">{{items.title}}</div>
          <div>
             
          </div>
        </t-card>
        <div class="flex justify-center mt-3">
                <t-button size="sm" variant="danger" @click="deletebook(items.id)">delete</t-button>
        </div>
       
        </div>
       

      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      favoriteBook: []
    }
  },
  mounted() {
    this.favoriteBook = JSON.parse(localStorage.getItem('Storebook') || '[]')
  },
  methods:{
      bookdetail(bookId) {
      this.$router.push({ name: 'Book-book', params: { book: bookId } })
    },
    deletebook(id){
        this.favoriteBook = this.favoriteBook.filter(bookid => bookid.id != id);
        localStorage.setItem('Storebook', JSON.stringify(this.favoriteBook))
    }
  }
}
</script>


<style  scoped>
.cut-text { 
  text-overflow: ellipsis;
  overflow: hidden; 
  width: 160px; 
  height: 1.2em; 
  white-space: nowrap;
}
</style>