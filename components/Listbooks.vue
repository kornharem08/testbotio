<template>
  <div class="grid grid-cols-5 gap-3">
    <t-card
      v-for="items in TheListBooks"
      :key="items.id"
      class="col-3"
      style="width:200px;height:250px; margin:25px"
      @click.native="bookdetail(items.id)"
    >
      <img v-if="items.volumeInfo.imageLinks" :src="items.volumeInfo.imageLinks.thumbnail" style="height:180px;width:150px"/>
      <img v-else src="https://semantic-ui.com/images/wireframe/image.png" style="height:180px;width:150px" />
      <div class="cut-text text-xs text-center mt-2">{{items.volumeInfo.title}}</div>
    </t-card>
  </div>
</template>


<script>
export default {
  props: {
    TheListBooks: {
      type: Array,
      default: []
    }
  },methods:{
      bookdetail(bookId) {
      this.$router.push({ name: 'Book-book', params: { book: bookId } })
    },
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