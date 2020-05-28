<template>
  <div>
    <div class="flex justify-end">
      <nuxt-link to="/favoriteBook">
        <t-button size="sm" class="bg-teal-400 mr-4 my-5 text-white">My Favorite</t-button>
      </nuxt-link>
    </div>
    <div class="grid grid-cols-12">
      <div class="col-span-1">
        <div>
          <div class="ml-4">Filter</div>
          <label v-for="option in options" :key="option.text" class="flex py-1">
            <t-radio
              :key="option.text"
              v-model="selectOption"
              :value="option.text"
              class="custom-radio"
            />
            <span class="ml-3">{{ option.text }}</span>
          </label>
        </div>
        <div>
          <div class="ml-4 mt-4">Print Type</div>
          <label v-for="option in optionPrintType" :key="option.text" class="flex py-1">
            <t-radio
              :key="option.text"
              v-model="slectOptionPrintType"
              :value="option.text"
              class="custom-radio"
            />
            <span class="ml-3">{{ option.text }}</span>
          </label>
        </div>
        <div class="mt-5 ml-2">
          <t-button class="bg-teal-400 text-white" style="width:30px;font-size:10px" @click="searchFilter">Apply</t-button>
          <t-button style="width:30px;font-size:10px" @click="clear">Clear</t-button>
        </div>
      </div>
      <div class="col-span-11">
        <div>
          <div class="mx-4">
            <t-input class="w-full" v-model="QueryStr" name="my-input" @keydown.enter="search" />
          </div>
          <div class="flex justify-end">
            <t-select
              class="w-32 mr-4 mt-3"
              v-model="orderby"
              name="orderby-input"
              @change="searchByOrder"
              :options="[
              { value: 'relevance', text: 'relevance' },
              { value: 'newest ', text: 'newest' }
                        ]"
            />
          </div>
        </div>
        <div>
          <Listbooks :TheListBooks="ListBooks" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Listbooks from '~/components/Listbooks'

export default {
  components: {
    Listbooks
  },
  data() {
    return {
      QueryStr: '',
      orderby: 'relevance',
      ListBooks: [],
      options: [
        { text: 'partial' },
        { text: 'full' },
        { text: 'free-ebooks' },
        { text: 'paid-ebooks' },
        { text: 'ebooks' }
      ],
      selectOption: '',
      optionPrintType: [
        { text: 'all' },
        { text: 'books' },
        { text: 'magazines' }
      ],
      slectOptionPrintType: ''
    }
  },
  methods: {
    async search() {
      const url = `https://www.googleapis.com/books/v1/volumes?q=${this.QueryStr}`
      await this.$axios.$get(url).then(response => {
        this.ListBooks = response.items
        this.orderby = 'relevance'
      })
    },
    async searchByOrder() {
       if(this.QueryStr != ''){
      const url = `https://www.googleapis.com/books/v1/volumes?q=${this.QueryStr}&orderBy=${this.orderby}`
      await this.$axios.$get(url).then(response => {
        this.ListBooks = response.items
      })
       }
    },
    clear() {
      this.slectOptionPrintType = ''
      this.selectOption = ''
    },
    async searchFilter() {
      if(this.QueryStr != ''){
        if (this.slectOptionPrintType != '' && this.selectOption == '') {
        const url = `https://www.googleapis.com/books/v1/volumes?q=${this.QueryStr}&printType=${this.slectOptionPrintType}`
        await this.$axios.$get(url).then(response => {
          this.ListBooks = response.items
       
        })
      }
      if (this.slectOptionPrintType == '' && this.selectOption != '') {
        const url = `https://www.googleapis.com/books/v1/volumes?q=${this.QueryStr}&filter=${this.selectOption}`
        await this.$axios.$get(url).then(response => {
          this.ListBooks = response.items
       
        })
      }

      if (this.slectOptionPrintType != '' && this.selectOption != '') {
        const url = `https://www.googleapis.com/books/v1/volumes?q=${this.QueryStr}&filter=${this.selectOption}&printType=${this.slectOptionPrintType}`
        await this.$axios.$get(url).then(response => {
          this.ListBooks = response.items
        
        })
      }
      }
      
    }
  }
}
</script>


<style  scoped>
input[type='radio'] {
  position: absolute;
  left: -9999px;
}
label input[type='radio'] + span {
  position: relative;
  padding: 0 0 0 30px;
  margin-top: 10px;
  cursor: pointer;
  font-size: 13px;
}
label input[type='radio'] + span:before {
  content: '';
  background: #fff;
  border: 2px solid #cbd5e0;
  height: 20px;
  width: 20px;
  border-radius: 50%;
  position: absolute;
  top: 2px;
  left: 0;
}
label input[type='radio'] + span:after {
  content: '';
  background: #37b34a;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  position: absolute;
  top: 7px;
  left: 5px;
  opacity: 0;
}
label input[type='radio']:checked + span:after {
  opacity: 1;
}
</style>
