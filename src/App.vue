<template>
  <div class="container">
    <the-header></the-header>
    <div class="form_wrapper">
      <add-new-expense @addNewScore="addRecord"></add-new-expense>
      <show-grossbux @showlist="showGrossbux"></show-grossbux>
    </div>    
    <list-of-costs :list="viewrecord"></list-of-costs>
    <choose-page :quantity="pageCount" @activePage="pageIdx = $event"></choose-page>
  </div>
</template>

<script>
import ListOfCosts from './components/ListOfCosts.vue'
import AddNewExpense from './components/AddNewExpense.vue'
import TheHeader from './components/TheHeader.vue'
import ChoosePage from './components/ChoosePage.vue'
import ShowGrossbux from './components/ShowGrossbux.vue'

const GET_URL = 'http://localhost:3030/mock';
const POST_URL = 'http://localhost:3030/addtomock';

export default {
  name: 'App',
  data() {
    return {
      grossbux:[],
      pageIdx: 0,
      loading:false,
      sorting: '1',
      lineCount: 10
    }
  },
  mounted(){    
  },
  methods: {
    async addRecord(record){
      try {        
        const response = await fetch(POST_URL, {
          method: 'POST',
          headers: {
              'Content-Type': 'application/json; charset=UTF-8'
              //;charset=utf-8'              
          },
          body: JSON.stringify(record)
        })
        const result = await response
        if (result.status === 200) this.grossbux.push(record)
      } catch(e){
        console.log(e.message);
      }      
    },
    async getRecods(){
      try {
        this.loading = true
        const response = await fetch(GET_URL)
        const data = await response.json();
        if(!data){
          throw new Error('Список персон пуст')
        } else {
          this.grossbux = [...data]
        }
        this.loading = false
      } catch(e) {
        console.log(e.message);
        this.loading = false
      }    
    },
    showGrossbux(sorting){
      this.sorting = sorting
      this.getRecods()
    }
  },
  computed: {    
    viewrecord(){
      let start = this.pageIdx * this.lineCount
      let end = start + this.lineCount
      if (this.sorting === '1') {
        return this.grossbux.slice(start, end)
      } else {
        return [...this.grossbux].reverse().slice(start, end)
      }      
    },
    pageCount(){
      return Math.ceil(this.grossbux.length / this.lineCount)
    }
  },
  components: {
    TheHeader,
    AddNewExpense,
    ListOfCosts,
    ChoosePage,
    ShowGrossbux    
  },
}
</script>