<script setup>
import { ref } from 'vue'
import GoalItem from './GoalItem.vue'
import {computed} from 'vue'
import GoalItem from './GoalItem.vue'



const totalGoals = computed(()=>goals.value.length)

const goals = ref([
  { text: 'Learn Vue Components', rating: 5 },
  { text: 'Upload to GitHub', rating: 4 }
])

const newGoalText = ref('')
const newGoalRating = ref(1)

function addGoal() {
  if (newGoalText.value) {
    goals.value.push({ text: newGoalText.value, rating: newGoalRating.value })
    newGoalText.value = ''
  }
}

function removeGoal(index) {
  goals.value.splice(index, 1)
}
</script>

<template>

  <ul v-if ="goal.length>0">
  <GoalItem   
  v-for ="(goal,index) in goals"
  :key="index"
  :goal="goal"
  @delete="removeGoal(index)"
  />

  </ul>
  <p v-else style ="color:gray;text-align:center;">NO GOALS ADD NEW GOALS</p>
  <div class="card">
    <h1>My Coding Goals</h1>
    <p>{totalGoals}</p>
    
    <!-- Input Section -->
    <div class="input-group">
      <input v-model="newGoalText" placeholder="New goal..." />
      <input v-model.number="newGoalRating" type="number" min="1" max="5" />
      <button @click="addGoal">Add</button>
    </div>

    <!-- Goal List using Component -->
    <GoalItem
      v-for="(goal, index) in goals"
      :key="index"
      :goal="goal"
      @delete="removeGoal(index)"
    />
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
button { padding: 8px 12px; cursor: pointer; }
input { padding: 8px; border: 1px solid #ccc; border-radius: 4px; }
</style>