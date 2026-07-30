<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import GoalItem from './GoalItem.vue'

const goals = ref([])
const newGoalText = ref('')
const newGoalRating = ref(1)
const errorMessage = ref('') // 1. የኤረር መልእክት ማከማቻ

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
  // 2. Validation: ጽሁፉ ባዶ ከሆነ ኤረር አሳይ
  if (!newGoalText.value.trim()) {
    errorMessage.value = 'እባክዎ ትክክለኛ ጎል ያስገቡ!'
    return
  }

  errorMessage.value = '' // ኤረር ከሌለ እናጸዳዋለን
  goals.value.push({ text: newGoalText.value, rating: newGoalRating.value })
  newGoalText.value = ''
}

function removeGoal(index) {
  goals.value.splice(index, 1)
}

const totalGoals = computed(() => goals.value.length)
</script>

<template>
  <div class="card">
    <h1>My Coding Goals</h1>
    
    <div class="input-group">
      <input v-model="newGoalText" placeholder="New goal..." />
      <input v-model.number="newGoalRating" type="number" min="1" max="5" />
      <button @click="addGoal">Add</button>
    </div>

    <!-- 3. ኤረር ካለ እዚህ እናሳያለን -->
    <p v-if="errorMessage" style="color: red; font-size: 14px; margin-top: -10px; margin-bottom: 10px;">
      {{ errorMessage }}
    </p>

    <p>Total Goals: {{ totalGoals }}</p>

    <ul v-if="goals.length > 0">
      <GoalItem
        v-for="(goal, index) in goals"
        :key="index"
        :goal="goal"
        @delete="removeGoal(index)"
      />
    </ul>
    <p v-else style="color: gray; text-align: center;">No Goals! Add New!</p>
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