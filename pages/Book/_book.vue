<template>
  <div>
    <div v-if="DataLoaded">
      <div>
      <TheBtn/>
        <div>
          <Book :bookDetail="bookDetail" />
        </div>
      </div>
    </div>
    <div v-else>
      <div class="flex justify-center items-center mt-64">
        <div class="lds-ring w-full">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TheBtn from '~/components/TheBtn'
import Book from '~/components/Bookdetail'

export default {
  components: {
    TheBtn,
    Book
  },
  data() {
    return {
      id: this.$route.params.book,
      bookDetail: [],
      setBook: [],
      DataLoaded: false,
    }
  },
  created() {
    this.setBook = JSON.parse(localStorage.getItem('Storebook') || '[]')
  },
  mounted() {
    const url = `https://www.googleapis.com/books/v1/volumes/${this.id}`
    this.$axios.$get(url).then(response => {
      this.bookDetail = response
      this.DataLoaded = true
    })
  },
  
}
</script>

<style scoped>
.lds-ring {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 64px;
  height: 64px;
  margin: 8px;
  border: 8px solid black;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: black transparent transparent transparent;
}
.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}
.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}
.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>