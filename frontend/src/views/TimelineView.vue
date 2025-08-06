<script setup>
import { ref, computed } from 'vue'
import { useTasks } from '../composables/useTasks'
import { toYYYYMMDD } from '../utils/date'
import { getTimelineTaskClass } from '../utils/taskUtils'
import { timeToMinutes, minutesToTime } from '../utils/time'

const props = defineProps({
  currentDate: {
    type: String,
    required: true,
  },
})

const emit = defineEmits(['change-date'])

const { tasks, editTask } = useTasks()

const selectedDate = ref(props.currentDate)
const draggedTask = ref(null)
const dragStartX = ref(0)
const originalStartMinutes = ref(0)

const onDateChange = () => {
  emit('change-date', selectedDate.value)
}

const handleMouseDown = (event, task) => {
  draggedTask.value = task
  dragStartX.value = event.clientX
  originalStartMinutes.value = timeToMinutes(task.start)

  window.addEventListener('mousemove', handleMouseMove)
  window.addEventListener('mouseup', handleMouseUp)
}

const handleMouseMove = (event) => {
  if (!draggedTask.value) return

  const deltaX = event.clientX - dragStartX.value
  const deltaMinutes = Math.round(deltaX) // 1px = 1 minute

  const taskDuration =
    timeToMinutes(draggedTask.value.end) -
    timeToMinutes(draggedTask.value.start)
  let newStartMinutes = originalStartMinutes.value + deltaMinutes

  // Constrain to day boundaries
  if (newStartMinutes < 0) newStartMinutes = 0
  if (newStartMinutes + taskDuration > 1440)
    newStartMinutes = 1440 - taskDuration

  draggedTask.value.start = minutesToTime(newStartMinutes)
  draggedTask.value.end = minutesToTime(newStartMinutes + taskDuration)
}

const handleMouseUp = () => {
  if (draggedTask.value) {
    editTask(draggedTask.value)
  }

  draggedTask.value = null
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('mouseup', handleMouseUp)
}

const tasksForSelectedDate = computed(() => {
  return tasks.value
    .filter(
      (task) => task.date === selectedDate.value && task.start && task.end
    )
    .sort((a, b) => a.start.localeCompare(b.start))
})

const getTaskStyle = (task) => {
  const startTotalMinutes = timeToMinutes(task.start)
  const endTotalMinutes = timeToMinutes(task.end)

  const topPosition =
    tasksForSelectedDate.value.findIndex((t) => t.id === task.id) * 60 + 20

  return {
    left: `${startTotalMinutes}px`,
    width: `${endTotalMinutes - startTotalMinutes}px`,
    top: `${topPosition}px`,
    cursor: 'grab',
  }
}

const taskEmojis = {
  Work: '💼',
  Rest: '😴',
  Study: '📚',
  Personal: '🏠',
  Prayer: '🙏',
  Activity: '🏃',
}

const getTaskEmoji = (type) => {
  return taskEmojis[type] || '📝'
}
</script>

<template>
  <div class="space-y-6">
    <div
      class="text-center glass-effect p-6 rounded-xl border border-blue-400/20"
    >
      <h2
        class="text-3xl font-bold bg-gradient-to-r from-blue-300 to-white bg-clip-text text-transparent mb-4"
      >
        Timeline View
      </h2>
      <input
        type="date"
        v-model="selectedDate"
        @change="onDateChange"
        class="px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
      />
    </div>

    <div
      class="glass-effect rounded-xl border border-blue-400/20 overflow-hidden"
    >
      <div class="relative h-[600px] overflow-x-auto">
        <div
          class="relative min-w-[700px] w-[1440px] h-full bg-gradient-to-b from-slate-800/30 to-slate-700/30"
        >
          <div
            v-for="hour in 24"
            :key="hour"
            class="absolute h-full border-r border-slate-600/30 text-xs"
            :style="{ left: `${(hour - 1) * 60}px` }"
          >
            <div
              class="absolute -top-6 left-1 px-2 py-1 bg-slate-700/50 rounded text-xs text-slate-300 font-medium"
            >
              {{ String(hour - 1).padStart(2, '0') }}:00
            </div>
            <div
              class="absolute top-0 left-0 w-full h-full border-r border-slate-600/20"
            ></div>
          </div>

          <div
            v-for="task in tasksForSelectedDate"
            :key="task.id"
            class="timeline-task absolute h-12 rounded-xl p-3 text-slate-100 text-sm font-medium shadow-lg border border-opacity-30 cursor-grab hover:scale-105 transition-all duration-300 group"
            :style="getTaskStyle(task)"
            :class="getTimelineTaskClass(task.type)"
            @mousedown="handleMouseDown($event, task)"
          >
            <div class="flex items-center gap-2 h-full">
              <span class="text-lg">{{ getTaskEmoji(task.type) }}</span>
              <span class="truncate group-hover:text-white">{{
                task.title
              }}</span>
            </div>
            <div
              class="absolute inset-0 bg-gradient-to-r from-white/10 to-transparent opacity-0 group-hover:opacity-100 rounded-xl transition-opacity duration-300"
            ></div>
          </div>

          <div
            v-if="tasksForSelectedDate.length === 0"
            class="flex items-center justify-center h-full"
          >
            <div class="text-center">
              <div class="text-6xl mb-4 opacity-50">📅</div>
              <p class="text-slate-300 text-lg font-medium">
                No scheduled tasks
              </p>
              <p class="text-slate-400 text-sm mt-2">
                Tasks with start and end times will appear here
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.glass-effect {
  background: rgba(26, 32, 44, 0.8);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(59, 130, 246, 0.2);
}

.timeline-task {
  min-width: 120px;
  max-width: 300px;
  overflow: hidden;
  z-index: 10;
}

.timeline-task:hover {
  z-index: 20;
}

input[type='date']::-webkit-calendar-picker-indicator {
  filter: invert(1);
  cursor: pointer;
}
</style>
