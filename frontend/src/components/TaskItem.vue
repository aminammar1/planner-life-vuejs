<script setup>
import { computed } from 'vue'
import { taskEmojis, getTaskTypeClass } from '../utils/taskUtils'

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
})

defineEmits(['delete', 'edit', 'toggle-completed'])
</script>

<template>
  <div
    class="p-4 glass-effect rounded-xl shadow-lg flex justify-between items-center group hover:shadow-2xl hover:shadow-blue-500/20 transition-all duration-300 border border-blue-400/20 relative overflow-hidden"
    :class="getTaskTypeClass(task.type)"
  >
    <div
      class="absolute inset-0 bg-gradient-to-r from-blue-600/5 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"
    ></div>

    <div class="flex items-center w-full relative z-10">
      <div class="relative mr-4">
        <input
          type="checkbox"
          class="h-5 w-5 rounded border-2 border-blue-400/50 bg-transparent checked:bg-gradient-to-r checked:from-blue-500 checked:to-blue-600 checked:border-blue-500 focus:ring-2 focus:ring-blue-500/50 transition-all duration-300"
          :checked="task.completed"
          @change="$emit('toggle-completed', task.id)"
        />
        <div
          class="absolute inset-0 rounded bg-blue-500/20 scale-0 group-hover:scale-110 transition-transform duration-300"
        ></div>
      </div>

      <div class="text-3xl mr-4 animate-float">{{ taskEmojis[task.type] }}</div>

      <div
        :class="task.completed ? 'opacity-60 line-through' : 'opacity-100'"
        class="transition-all duration-300"
      >
        <h3
          class="font-bold text-slate-100 text-lg group-hover:text-white transition-colors duration-300"
        >
          {{ task.title }}
          <span v-if="task.rating" class="text-yellow-400 ml-2 drop-shadow-lg">
            ({{ task.rating }} ★)
          </span>
        </h3>
        <p class="text-sm text-blue-200/80 font-medium">
          {{ task.start }} - {{ task.end }}
        </p>
        <p
          v-if="task.description"
          class="text-sm text-slate-300 mt-1 opacity-90"
        >
          {{ task.description }}
        </p>
      </div>
    </div>

    <div class="flex items-center space-x-3 relative z-10">
      <button
        @click="$emit('edit', task)"
        class="text-blue-300 hover:text-blue-100 cursor-pointer p-2 rounded-lg hover:bg-blue-600/20 transition-all duration-300 group/btn"
        title="Edit task"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 group-hover/btn:scale-110 transition-transform duration-300"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            d="M13.586 3.586a2 2 0 112.828 2.828l-8 8a2 2 0 01-1.414.586H4a2 2 0 01-2-2v-2.172a2 2 0 01.586-1.414l8-8zM12 5l-8 8v1h1l8-8-1-1z"
          />
        </svg>
      </button>

      <button
        @click="$emit('delete', task.id)"
        class="text-slate-400 hover:text-red-400 cursor-pointer p-2 rounded-lg hover:bg-red-600/20 transition-all duration-300 group/btn"
        title="Delete task"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 group-hover/btn:scale-110 transition-transform duration-300"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm4 0a1 1 0 012 0v6a1 1 0 11-2 0V8z"
            clip-rule="evenodd"
          />
        </svg>
      </button>
    </div>
  </div>
</template>

<style scoped>
.glass-effect {
  background: rgba(26, 32, 44, 0.8);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(59, 130, 246, 0.2);
}

.animate-float {
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-6px);
  }
}

input[type='checkbox'] {
  appearance: none;
  -webkit-appearance: none;
  position: relative;
}

input[type='checkbox']:checked::before {
  content: '✓';
  position: absolute;
  top: -2px;
  left: 1px;
  color: white;
  font-size: 14px;
  font-weight: bold;
}
</style>
