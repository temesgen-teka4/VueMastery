<script setup>
// Props: ከወላጅ የምንቀበለው መረጃ
defineProps(['goal'])

// Emits: ለወላጅ የምንልክለት ምልክት
// ስህተቱ እዚህ ነበር -> () መጨመር አለበት
const emit = defineEmits(['delete'])
</script>

<template>
  <div class="goal-item">
    <span>{{ goal.text }} (⭐{{ goal.rating }})</span>
    <!-- Delete ሲነካ ለወላጁ 'delete' የሚል ምልክት እንልካለን -->
    <button @click="emit('delete')" class="del-btn">Delete</button>
  </div>
</template>

<style scoped>
.goal-item { 
  display: flex; 
  justify-content: space-between; 
  padding: 10px; 
  border-bottom: 1px solid #ddd; 
}
.del-btn { 
  background: #ff4d4d; 
  color: white; 
  border: none; 
  border-radius: 4px; 
  cursor: pointer; 
}
</style>