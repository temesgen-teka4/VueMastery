<script setup>
defineProps({
  goal: Object
})

defineEmits(['delete', 'toggle'])
</script>

<template>
  <li class="goal-item">
    <!-- ጽሁፉን ስንጫነው toggle ፋንክሽን ይጠራል፣ completed ከነቃም line-through ይሆናል -->
    <span 
      @click="$emit('toggle')" 
      :class="{ completed: goal.completed }"
      style="cursor: pointer; flex-grow: 1;"
    >
      {{ goal.text }} (⭐ {{ goal.rating }})
    </span>
    
    <button @click="$emit('delete')" class="delete-btn">Delete</button>
  </li>
</template>

<style scoped>
.goal-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #231f65;
  margin-bottom: 8px;
  padding: 8px 12px;
  border-radius: 6px;
  color: white;
}

/* ጎሉሲጠናቀቅ የሚመታ መስመር እና የቀለም ማስተካከያ */
.completed {
  text-decoration: line-through;
  color: #9ca3af;
}

.delete-btn {
  background-color: #ef4444;
  color: white;
  border: none;
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
}
.delete-btn:hover {
  background-color: #dc2626;
}
</style>