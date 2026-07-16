<script setup>
import { ref } from 'vue'
import GoalItem from './component/GoalItem.vue' // import the file 



const goals = ref([
  { text: 'Learn Vue Components', rating: 5 },
  { text: 'Upload to GitHub', rating: 4 }
])

function removeGoal(index){
  goals.value.splice(index,1)
}

const newGoalText = ref('')
const newGoalRating = ref(1)

function addGoal() {
  if (newGoalText.value) {
    goals.value.push({ 
      text: newGoalText.value, 
      rating: newGoalRating.value 
    })
    newGoalText.value = ''
  }
}


</script>

<template>
  <div class="card">
    <h1>My Coding Goals</h1>
    
    <div class="card">
      <h1>My Goals</h1>
      // We gonna loop it 
      <GoalItem
      v-for ="(goal,index) in goals"
      :key="index"
      :goal="goal"
      @delete ="removeGoal(index)"
      />
    </div>

    <ul>
      <li v-for="(goal, index) in goals" :key="index" class="goal-item">
        <span>{{ goal.text }} (⭐ {{ goal.rating }})</span>
        <button @click="deleteGoal(index)" class="del-btn">Delete</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.card {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  background: #f9f9f9;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  font-family: sans-serif;
}

.input-group { display: flex; gap: 5px; margin-bottom: 20px; }

.goal-item {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.del-btn { background: #ff4d4d; color: white; border: none; border-radius: 4px; cursor: pointer; }
input { padding: 8px; border: 1px solid #ccc; border-radius: 4px; }
button { padding: 8px 12px; cursor: pointer; }
</style>