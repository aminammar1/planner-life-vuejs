<script setup>
import { getTaskCardClass } from '../utils/taskUtils'

const props = defineProps({
  day: {
    type: Object,
    required: true,
  },
  tasks: {
    type: Array,
    required: true,
  },
})

const taskEmojis = {
  work: '💼',
  personal: '🏠',
  health: '💪',
  learning: '📚',
  social: '👥',
  entertainment: '🎮',
  shopping: '🛒',
  travel: '✈️',
  finance: '💰',
  other: '📝',
}

const getTaskEmoji = (type) => {
  return taskEmojis[type] || taskEmojis.other
}
</script>

<template>
  <div
    class="glass-effect rounded-xl shadow-lg hover:shadow-2xl hover:shadow-blue-500/20 transition-all duration-300 p-6 group border border-blue-400/20 relative overflow-hidden"
  >
    <div
      class="absolute inset-0 bg-gradient-to-br from-blue-600/10 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"
    ></div>

    <div class="relative z-10">
      <div class="flex justify-between items-center mb-4">
        <h3
          class="font-bold text-lg text-slate-100 group-hover:text-white transition-colors duration-300"
        >
          {{ day.name }}
        </h3>
        <div
          class="flex items-center justify-center w-8 h-8 bg-gradient-to-r from-blue-500 to-blue-600 rounded-lg text-white font-bold text-sm shadow-lg"
        >
          {{ day.date.split('-')[2] }}
        </div>
      </div>

      <div class="space-y-2">
        <div
          v-for="task in tasks"
          :key="task.id"
          class="text-sm p-3 rounded-lg font-medium transition-all duration-300 hover:scale-105 border border-opacity-20"
          :class="getTaskCardClass(task.type)"
        >
          <div class="flex items-center gap-2">
            <span class="text-lg">{{ getTaskEmoji(task.type) }}</span>
            <span class="text-slate-100">{{ task.title }}</span>
          </div>
        </div>

        <div
          v-if="tasks.length === 0"
          class="text-slate-400 text-sm italic text-center py-4"
        >
          No tasks scheduled
        </div>
      </div>
    </div>
  </div>
</template>
