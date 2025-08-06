<script setup>
import { ref, computed } from 'vue'
import draggable from 'vuedraggable'
import { useTasks } from '../composables/useTasks'
import TaskItem from '../components/TaskItem.vue'
import Modal from '../components/Modal.vue'
import MoodTracker from '../components/MoodTracker.vue'
import ExpenseTracker from '../components/ExpenseTracker.vue'
import { toYYYYMMDD } from '../utils/date'

const props = defineProps({
  currentDate: {
    type: String,
    required: true,
  },
})
const emit = defineEmits(['change-date'])

const today = toYYYYMMDD(new Date())

const showMoodTracker = computed(() => {
  return props.currentDate <= today
})

const isMoodEditable = computed(() => {
  return props.currentDate <= today
})

const {
  tasks,
  addTask,
  editTask,
  deleteTask,
  updateTaskOrder,
  toggleTaskCompletion,
} = useTasks()
const showModal = ref(false)
const editingTask = ref(null)
const showCompleted = ref(false)

const tasksForDay = computed({
  get() {
    return tasks.value.filter((task) => task.date === props.currentDate)
  },
  set(newTasks) {
    const otherTasks = tasks.value.filter(
      (task) => task.date !== props.currentDate
    )
    updateTaskOrder([...otherTasks, ...newTasks])
  },
})

const visibleTasksForDay = computed(() => {
  return tasksForDay.value.filter(
    (task) => showCompleted.value || !task.completed
  )
})

const onDragEnd = () => {
  // The v-model on draggable handles the update, this is just a hook if needed.
  // The 'set' part of the computed property 'tasksForDay' will be triggered.
}

const formattedDate = computed(() => {
  const date = new Date(props.currentDate.replace(/-/g, '/'))
  return date.toLocaleDateString(undefined, {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  })
})

const openModal = (task = null) => {
  editingTask.value = task
  showModal.value = true
}

const closeModal = () => {
  editingTask.value = null
  showModal.value = false
}

const changeDay = (offset) => {
  const current = new Date(props.currentDate.replace(/-/g, '/'))
  current.setDate(current.getDate() + offset)
  emit('change-date', toYYYYMMDD(current))
}

const handleSaveTask = (task) => {
  const repeat = task.repeat || 'None'
  if (task._id || repeat === 'None') {
    if (task._id) {
      editTask(task)
    } else {
      addTask(task, props.currentDate)
    }
  } else {
    // Generate dates for the current week
    const baseDate = new Date(props.currentDate.replace(/-/g, '/'))
    const weekStart = new Date(baseDate)
    weekStart.setDate(baseDate.getDate() - baseDate.getDay()) // Sunday
    for (let i = 0; i < 7; i++) {
      const d = new Date(weekStart)
      d.setDate(weekStart.getDate() + i)
      const day = d.getDay()
      if (
        repeat === 'Every Day' ||
        (repeat === 'Weekdays Only' && day >= 1 && day <= 5) ||
        (repeat === 'Weekends Only' && (day === 0 || day === 6))
      ) {
        const isCurrent = toYYYYMMDD(d) === props.currentDate
        addTask(
          {
            ...task,
            repeat: undefined,
            description: isCurrent ? task.description : '',
          },
          toYYYYMMDD(d)
        )
      }
    }
  }
  closeModal()
}
</script>

<template>
  <div class="space-y-6">
    <div class="flex justify-between items-center">
      <button
        @click="changeDay(-1)"
        class="px-4 py-3 glass-effect rounded-xl border border-blue-400/30 cursor-pointer flex items-center justify-center hover:bg-blue-600/20 transition-all duration-300 group"
      >
        <svg
          class="w-5 h-5 sm:hidden text-blue-300 group-hover:text-blue-100"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M15 19l-7-7 7-7"
          />
        </svg>
        <span
          class="hidden sm:inline text-white font-medium group-hover:text-blue-200"
          >← Previous</span
        >
      </button>

      <div
        class="text-center glass-effect px-8 py-4 rounded-xl border border-blue-400/30"
      >
        <h2
          class="text-2xl font-bold bg-gradient-to-r from-blue-300 to-white bg-clip-text text-transparent"
        >
          Day View
        </h2>
        <p class="text-blue-200 font-medium mt-1">{{ formattedDate }}</p>
      </div>

      <button
        @click="changeDay(1)"
        class="px-4 py-3 glass-effect rounded-xl border border-blue-400/30 cursor-pointer flex items-center justify-center hover:bg-blue-600/20 transition-all duration-300 group"
      >
        <span
          class="hidden sm:inline text-white font-medium group-hover:text-blue-200"
          >Next →</span
        >
        <svg
          class="w-5 h-5 sm:hidden text-blue-300 group-hover:text-blue-100"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M9 5l7 7-7 7"
          />
        </svg>
      </button>
    </div>

    <MoodTracker
      v-if="showMoodTracker"
      :date="currentDate"
      :is-editable="isMoodEditable"
    />

    <div
      class="flex items-center gap-3 glass-effect p-4 rounded-xl border border-blue-400/20"
    >
      <input
        type="checkbox"
        id="showCompleted"
        v-model="showCompleted"
        class="h-5 w-5 rounded border-2 border-blue-400/50 bg-transparent checked:bg-gradient-to-r checked:from-blue-500 checked:to-blue-600 checked:border-blue-500 focus:ring-2 focus:ring-blue-500/50 transition-all duration-300"
      />
      <label
        for="showCompleted"
        class="text-white font-medium select-none cursor-pointer"
      >
        Show completed tasks
      </label>
    </div>

    <div class="space-y-4">
      <draggable
        v-model="tasksForDay"
        tag="div"
        item-key="id"
        class="space-y-4"
        @end="onDragEnd"
      >
        <template #item="{ element: task }">
          <TaskItem
            v-if="showCompleted || !task.completed"
            :task="task"
            @edit="openModal(task)"
            @delete="deleteTask(task._id)"
            @toggle-completed="toggleTaskCompletion"
          />
        </template>
      </draggable>

      <div v-if="visibleTasksForDay.length === 0" class="text-center py-12">
        <div class="glass-effect rounded-xl p-8 border border-blue-400/20">
          <div class="text-6xl mb-4 opacity-50">📝</div>
          <p class="text-white text-lg font-medium">No tasks for today</p>
          <p class="text-blue-200 text-sm mt-2">
            Click the + button to add your first task
          </p>
        </div>
      </div>
    </div>

    <button
      @click="openModal()"
      class="fixed bottom-6 right-6 w-16 h-16 bg-gradient-to-r from-blue-500 to-blue-600 text-white rounded-full shadow-2xl shadow-blue-500/30 hover:shadow-blue-500/50 hover:scale-110 transition-all duration-300 flex items-center justify-center group z-20"
    >
      <svg
        class="w-8 h-8 group-hover:rotate-180 transition-transform duration-300"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        viewBox="0 0 24 24"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M12 4v16m8-8H4"
        />
      </svg>
    </button>

    <Modal
      v-if="showModal"
      :task="editingTask"
      @close="closeModal"
      @save="handleSaveTask"
    />

    <ExpenseTracker :date="currentDate" />
  </div>
</template>

<style scoped></style>
