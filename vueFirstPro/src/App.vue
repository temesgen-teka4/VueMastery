<script setup>
import {ref} from 'vue'

const count = ref(0) // this is our rreactive varible 

function increment (){
  count.value++ // Note :we must use .value in the script 
 }
</script>

<template>
  <div class ="container">
  <h1> Counter:{{count}} </h1>
  <button @click="increment">Add+1</button>
  </div>
</template>

<style scoped>
.container{
text-align:center;
margin-top:50px;
background-color:aqua
}
button{
padding :10px 20px; 
font-size:20px;
}

</style>
