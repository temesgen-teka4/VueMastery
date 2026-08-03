<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import GoalItem from './GoalItem.vue'

const goals = ref([])
const newGoalText = ref('')
const newGoalRating = ref(1)
const errorMessage = ref('') 
const searchQuery = ref('') 

onMounted(() => {
  const savedGoals = localStorage.getItem('goals')
  if (savedGoals) {
    goals.value = JSON.parse(savedGoals)
  }
})

watch(goals, (newValue) => {
  localStorage.setItem('goals', JSON.stringify(newValue))
}, { deep: true })

function addGoal() {
  if (!newGoalText.value.trim()) {
    errorMessage.value = 'እባክዎ ትክክለኛ ጎል ያስገቡ!'
    return
  }

  errorMessage.value = ''
  goals.value.push({ text: newGoalText.value, rating: newGoalRating.value })
  newGoalText.value = ''
}

function removeGoal(index) {
  const target = filteredGoals.value[index]
  const originalIndex = goals.value.indexOf(target)
  if (originalIndex > -1) {
    goals.value.splice(originalIndex, 1)
  }
}

const totalGoals = computed(() => goals.value.length)

// አዲስ: የደረጃዎች አማካይ (Average Rating) የሚያሰላ computed
const averageRating = computed(() => {
  if (goals.value.length === 0) return 0
  const sum = goals.value.reduce((acc, goal) => acc + Number(goal.rating), 0)
  return (sum / goals.value.length).toFixed(1)
})

const filteredGoals = computed(() => {
  return goals.value.filter(goal => 
    goal.text.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <div class="card">
    <h1 class="h1">My Coding Goals</h1>
    
    <div class="input-group">
      <input v-model="newGoalText" placeholder="New goal..." />
      <input v-model.number="newGoalRating" type="number" min="1" max="5" />
      <button @click="addGoal">Add</button>
    </div>

    <div class="input-group" style="margin-top: 10px;">
      <input v-model="searchQuery" placeholder="Search goals..." style="width: 100%;" />
    </div>

    <p v-if="errorMessage" style="color: red; font-size: 14px; margin-top: -5px; margin-bottom: 10px;">
      {{ errorMessage }}
    </p>

    <!-- የስታትስቲክስ ማሳያ (Total & Average) -->
    <div v-else style="display: flex; justify-content: space-between; color: aliceblue; margin-bottom: 15px; font-size: 14px;">
      <p>Total Goals: {{ totalGoals }}</p>
      <p>Avg Rating: ⭐ {{ averageRating }}</p>
    </div>

    <ul v-if="filteredGoals.length > 0">
      <GoalItem
        v-for="(goal, index) in filteredGoals"
        :key="index"
        :goal="goal"
        @delete="removeGoal(index)"
      />
    </ul>
    <p v-else style="color: gray; text-align: center;">No Goals Found!</p>
  </div>
</template>

<style scoped>
.h1 {
  color:rgb(218, 222, 225);
  font-size:x-large;
}
.card {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  background: #18144b;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(107, 14, 14, 0.1);
  font-family: sans-serif;
}
.input-group { display: flex; gap: 5px; margin-bottom: 20px; color:rgb(220, 235, 235) }
button { padding: 8px 12px; cursor: pointer; }
input { padding: 8px; border: 1px solid #6d8ec0; border-radius: 4px; }
</style>