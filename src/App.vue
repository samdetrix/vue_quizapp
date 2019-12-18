<template>
  <div id="app">
    <Header 
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container>
      <b-row class="col-6 offset-3">
        <b-col>
          <Box
          v-if="questions.length"
          :currentQuestion="questions[index]"
          :next="next"
          :increament="increament"
           />
        </b-col>        
      </b-row>
</b-container>
    
  </div>
</template>

<script>
import Box from './components/Box.vue'
import Header from './components/Header'



export default {
  name: 'app',
  components: {
    Box,
    Header
  },
  data() {
    return{
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  

  methods:{
    next(){
      this.index++
    },
    increament(isCorrect){
      if(isCorrect){
        this.numCorrect++
        }
      this.numTotal++
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=26&type=multiple', {
      method: 'get'     
    })
    .then((Response)=>{
      return Response.json()
    })
    .then((jsonData)=>{
      this.questions=jsonData.results
    })
  }
  
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
