<script setup>
import { ref, computed } from 'vue'
import { useTasks } from '../composables/useTasks'
import { useMoods } from '../composables/useMoods'
import { toYYYYMMDD } from '../utils/date'
import { getTaskCardClass } from '../utils/taskUtils'
import { useExpenses } from '../composables/useExpenses'

const { tasks } = useTasks()
const { moods } = useMoods()
const { getExpensesByDate, getTotalByMonth } = useExpenses()
const today = toYYYYMMDD(new Date())

const tasksByDate = computed(() => {
  return tasks.value.reduce((acc, task) => {
    ;(acc[task.date] = acc[task.date] || []).push(task)
    return acc
  }, {})
})

const moodEmojis = {
  1: '😞',
  2: '😐',
  3: '🙂',
  4: '😄',
  5: '😁',
}

const taskTypeClass = (type) => {
  switch (type) {
    case 'Work':
      return 'bg-blue-200 text-blue-800'
    case 'Rest':
      return 'bg-green-200 text-green-800'
    case 'Study':
      return 'bg-yellow-200 text-yellow-800'
    case 'Personal':
      return 'bg-purple-200 text-purple-800'
    case 'Prayer':
      return 'bg-indigo-200 text-indigo-800'
    case 'Activity':
      return 'bg-pink-200 text-pink-800'
    default:
      return 'bg-gray-200 text-gray-800'
  }
}

const currentDate = ref(new Date())

const year = computed(() => currentDate.value.getFullYear())
const month = computed(() => currentDate.value.getMonth())
const monthName = computed(() =>
  currentDate.value.toLocaleDateString(undefined, { month: 'long' })
)
const dayNames = computed(() => {
  if (window.innerWidth < 768) {
    return ['M', 'T', 'W', 'T', 'F', 'S', 'S']
  }
  return ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
})

const days = computed(() => {
  const firstDayOfMonth = new Date(year.value, month.value, 1)
  const lastDayOfMonth = new Date(year.value, month.value + 1, 0)
  const daysInMonth = lastDayOfMonth.getDate()
  const startDayOfWeek = (firstDayOfMonth.getDay() + 6) % 7 // 0 = Monday

  const daysArray = []

  // Days from previous month
  const prevMonthLastDay = new Date(year.value, month.value, 0).getDate()
  for (let i = startDayOfWeek - 1; i >= 0; i--) {
    daysArray.push({
      date: new Date(year.value, month.value - 1, prevMonthLastDay - i),
      dayOfMonth: prevMonthLastDay - i,
      isCurrentMonth: false,
    })
  }

  // Days in current month
  for (let i = 1; i <= daysInMonth; i++) {
    daysArray.push({
      date: new Date(year.value, month.value, i),
      dayOfMonth: i,
      isCurrentMonth: true,
    })
  }

  // Days from next month
  const endDayOfWeek = (lastDayOfMonth.getDay() + 6) % 7 // 0 = Monday
  for (let i = 1; i < 7 - endDayOfWeek; i++) {
    daysArray.push({
      date: new Date(year.value, month.value + 1, i),
      dayOfMonth: i,
      isCurrentMonth: false,
    })
  }

  return daysArray.map((d) => ({
    ...d,
    date: toYYYYMMDD(d.date),
  }))
})

const prevMonth = () => {
  currentDate.value = new Date(year.value, month.value - 1, 1)
}

const nextMonth = () => {
  currentDate.value = new Date(year.value, month.value + 1, 1)
}

const emit = defineEmits(['set-view-with-date'])

const selectDay = (day) => {
  if (day.isCurrentMonth) {
    emit('set-view-with-date', { view: 'DayView', date: day.date })
  }
}

const viewMode = ref('tasks') // 'tasks' or 'expenses'

const monthString = computed(
  () => `${year.value}-${String(month.value + 1).padStart(2, '0')}`
)
const monthTotal = computed(() => getTotalByMonth(monthString.value))
</script>

<template>
  <div class="space-y-6">
    <div class="flex justify-between items-center">
      <button
        @click="prevMonth"
        class="px-6 py-3 glass-effect rounded-xl border border-blue-400/30 cursor-pointer hover:bg-blue-600/20 transition-all duration-300 group"
      >
        <svg
          class="w-6 h-6 text-blue-300 group-hover:text-blue-100 group-hover:-translate-x-1 transition-all duration-300"
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
      </button>

      <div
        class="text-center glass-effect px-8 py-4 rounded-xl border border-blue-400/30"
      >
        <h2
          class="text-3xl font-bold bg-gradient-to-r from-blue-300 to-white bg-clip-text text-transparent"
        >
          {{ monthName }} {{ year }}
        </h2>
      </div>

      <button
        @click="nextMonth"
        class="px-6 py-3 glass-effect rounded-xl border border-blue-400/30 cursor-pointer hover:bg-blue-600/20 transition-all duration-300 group"
      >
        <svg
          class="w-6 h-6 text-blue-300 group-hover:text-blue-100 group-hover:translate-x-1 transition-all duration-300"
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

    <div class="flex justify-center">
      <div class="glass-effect rounded-xl p-2 border border-blue-400/20">
        <button
          :class="[
            'px-6 py-3 rounded-lg font-medium transition-all duration-300',
            viewMode === 'tasks'
              ? 'bg-gradient-to-r from-blue-500 to-blue-600 text-white shadow-lg'
              : 'text-slate-300 hover:text-white hover:bg-blue-600/20',
          ]"
          @click="viewMode = 'tasks'"
        >
          📋 Tasks
        </button>
        <button
          :class="[
            'px-6 py-3 rounded-lg font-medium transition-all duration-300',
            viewMode === 'expenses'
              ? 'bg-gradient-to-r from-blue-500 to-blue-600 text-white shadow-lg'
              : 'text-slate-300 hover:text-white hover:bg-blue-600/20',
          ]"
          @click="viewMode = 'expenses'"
        >
          💰 Expenses
        </button>
      </div>
    </div>

    <div
      v-if="viewMode === 'tasks'"
      class="glass-effect rounded-xl border border-blue-400/20 overflow-hidden"
    >
      <div class="overflow-x-auto">
        <div class="grid grid-cols-7 gap-px bg-slate-600/30 min-w-[700px]">
          <div
            v-for="dayName in dayNames"
            :key="dayName"
            class="py-4 text-center font-bold bg-gradient-to-r from-slate-700 to-slate-600 text-slate-200 border-r border-slate-500/30"
          >
            {{ dayName }}
          </div>

          <div
            v-for="day in days"
            :key="day.date"
            class="p-3 h-36 bg-slate-800/50 border-r border-b border-slate-600/30 overflow-y-auto cursor-pointer hover:bg-blue-600/10 transition-all duration-300 group"
            :class="{ 'bg-slate-700/30': !day.isCurrentMonth }"
            @click="selectDay(day)"
          >
            <div class="flex justify-between items-center mb-2">
              <div
                :class="[
                  'font-bold text-sm transition-all duration-300',
                  !day.isCurrentMonth
                    ? 'text-slate-500'
                    : day.date === today
                    ? 'text-white bg-gradient-to-r from-blue-500 to-blue-600 rounded-full h-8 w-8 flex items-center justify-center shadow-lg'
                    : 'text-slate-200 group-hover:text-white',
                ]"
              >
                {{ day.dayOfMonth }}
              </div>
              <div v-if="moods[day.date]" class="text-2xl animate-float">
                {{ moodEmojis[moods[day.date]] }}
              </div>
            </div>

            <div class="space-y-1">
              <div
                v-for="task in tasksByDate[day.date]"
                :key="task.id"
                class="text-xs p-2 rounded-lg font-medium border transition-all duration-300 hover:scale-105"
                :class="getTaskCardClass(task.type)"
              >
                {{ task.title }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-else class="space-y-6">
      <div
        class="glass-effect rounded-xl border border-blue-400/20 overflow-hidden"
      >
        <div class="overflow-x-auto">
          <div class="grid grid-cols-7 gap-px bg-slate-600/30 min-w-[700px]">
            <div
              v-for="dayName in dayNames"
              :key="dayName"
              class="py-4 text-center font-bold bg-gradient-to-r from-slate-700 to-slate-600 text-slate-200 border-r border-slate-500/30"
            >
              {{ dayName }}
            </div>

            <div
              v-for="day in days"
              :key="day.date"
              class="p-3 h-36 bg-slate-800/50 border-r border-b border-slate-600/30 overflow-y-auto transition-all duration-300"
              :class="{ 'bg-slate-700/30': !day.isCurrentMonth }"
            >
              <div class="flex justify-between items-center mb-2">
                <div
                  :class="[
                    'font-bold text-sm',
                    !day.isCurrentMonth
                      ? 'text-slate-500'
                      : day.date === today
                      ? 'text-white bg-gradient-to-r from-blue-500 to-blue-600 rounded-full h-8 w-8 flex items-center justify-center shadow-lg'
                      : 'text-slate-200',
                  ]"
                >
                  {{ day.dayOfMonth }}
                </div>
                <div
                  class="text-sm font-bold bg-gradient-to-r from-blue-300 to-blue-500 bg-clip-text text-transparent"
                >
                  {{
                    getExpensesByDate(day.date)
                      .reduce((sum, e) => sum + e.amount, 0)
                      .toFixed(2)
                  }}€
                </div>
              </div>

              <div class="space-y-1 max-h-20 overflow-y-auto">
                <div
                  v-for="expense in getExpensesByDate(day.date)"
                  :key="expense._id"
                  class="bg-slate-700/30 rounded p-2 border border-slate-600/30"
                >
                  <div class="text-xs text-slate-200 truncate">
                    {{ expense.description }}
                  </div>
                  <div class="text-xs text-red-400 font-bold">
                    {{ expense.amount.toFixed(2) }}€
                  </div>
                </div>
                <div
                  v-if="getExpensesByDate(day.date).length === 0"
                  class="text-center text-slate-500 italic text-xs py-2"
                >
                  No expenses
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        class="text-center glass-effect p-6 rounded-xl border border-blue-400/20"
      >
        <div class="text-slate-300 text-lg mb-2">Monthly Total</div>
        <div
          class="text-4xl font-bold bg-gradient-to-r from-blue-300 to-blue-500 bg-clip-text text-transparent"
        >
          {{ monthTotal.toFixed(2) }}€
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
</style>
