<script setup>
import { ref, computed } from 'vue'
import { useTasks } from '../composables/useTasks'
import DayCard from '../components/DayCard.vue'
import { toYYYYMMDD } from '../utils/date'
import { useExpenses } from '../composables/useExpenses'

const currentDate = ref(new Date())

const { tasks } = useTasks()
const { getExpensesByDate } = useExpenses()

const tasksByDate = computed(() => {
  return tasks.value.reduce((acc, task) => {
    ;(acc[task.date] = acc[task.date] || []).push(task)
    return acc
  }, {})
})

const week = computed(() => {
  const startOfWeek = new Date(currentDate.value)
  const currentDay = startOfWeek.getDay()
  const date = startOfWeek.getDate() - currentDay + (currentDay === 0 ? -6 : 1) // Adjust to start on Monday
  startOfWeek.setDate(date)

  const days = []
  for (let i = 0; i < 7; i++) {
    const date = new Date(startOfWeek)
    date.setDate(date.getDate() + i)
    days.push({
      name: date.toLocaleDateString(undefined, { weekday: 'short' }),
      date: toYYYYMMDD(date),
    })
  }
  return days
})

const viewMode = ref('tasks') // 'tasks' or 'expenses'

const weekExpenses = computed(() => {
  return week.value.map((day) => {
    const expenses = getExpensesByDate(day.date)
    const total = expenses.reduce((sum, e) => sum + e.amount, 0)
    return { date: day.date, total }
  })
})
const weekTotal = computed(() =>
  weekExpenses.value.reduce((sum, d) => sum + d.total, 0)
)

const emit = defineEmits(['set-view-with-date'])

const selectDay = (day) => {
  emit('set-view-with-date', { view: 'DayView', date: day.date })
}

const formatWeekRange = () => {
  const firstDay = week.value[0]
  const lastDay = week.value[6]
  const startDate = new Date(firstDay.date)
  const endDate = new Date(lastDay.date)

  return `${startDate.toLocaleDateString(undefined, {
    month: 'short',
    day: 'numeric',
  })} - ${endDate.toLocaleDateString(undefined, {
    month: 'short',
    day: 'numeric',
    year: 'numeric',
  })}`
}

const formatDate = (dateStr) => {
  const date = new Date(dateStr)
  return date.toLocaleDateString(undefined, { month: 'short', day: 'numeric' })
}
</script>

<template>
  <div class="space-y-6">
    <div
      class="text-center glass-effect p-6 rounded-xl border border-blue-400/20"
    >
      <h2
        class="text-3xl font-bold bg-gradient-to-r from-blue-300 to-white bg-clip-text text-transparent"
      >
        Week Overview
      </h2>
      <p class="text-blue-200 mt-2">{{ formatWeekRange() }}</p>
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

    <div v-if="viewMode === 'tasks'" class="space-y-4">
      <div class="grid grid-cols-1 md:grid-cols-7 gap-4">
        <DayCard
          v-for="day in week"
          :key="day.date"
          :day="day"
          :tasks="tasksByDate[day.date] || []"
          @click="selectDay(day)"
          class="cursor-pointer hover:scale-105 transition-transform duration-300"
        />
      </div>
    </div>

    <div v-else class="space-y-6">
      <div class="grid grid-cols-1 md:grid-cols-7 gap-4">
        <div
          v-for="day in week"
          :key="day.date"
          class="glass-effect p-6 rounded-xl border border-blue-400/20 flex flex-col items-center hover:shadow-lg hover:shadow-blue-500/20 transition-all duration-300"
        >
          <div class="font-bold text-slate-200 mb-3 text-lg">
            {{ day.name }}
          </div>
          <div
            class="text-3xl font-bold bg-gradient-to-r from-blue-300 to-blue-500 bg-clip-text text-transparent mb-2"
          >
            {{
              weekExpenses.find((d) => d.date === day.date)?.total.toFixed(2) ||
              '0.00'
            }}€
          </div>
          <div class="text-xs text-slate-400 mb-4">
            {{ formatDate(day.date) }}
          </div>

          <div class="w-full space-y-2">
            <div
              v-for="expense in getExpensesByDate(day.date)"
              :key="expense._id"
              class="bg-slate-700/30 rounded-lg p-3 border border-slate-600/30"
            >
              <div class="flex justify-between items-center">
                <span class="text-slate-200 text-sm">{{
                  expense.description
                }}</span>
                <span class="text-red-400 font-bold"
                  >{{ expense.amount.toFixed(2) }}€</span
                >
              </div>
            </div>
            <div
              v-if="getExpensesByDate(day.date).length === 0"
              class="text-center py-4 text-slate-400 italic"
            >
              No expenses
            </div>
          </div>
        </div>
      </div>

      <div
        class="text-center glass-effect p-6 rounded-xl border border-blue-400/20"
      >
        <div class="text-slate-300 text-lg mb-2">Weekly Total</div>
        <div
          class="text-4xl font-bold bg-gradient-to-r from-blue-300 to-blue-500 bg-clip-text text-transparent"
        >
          {{ weekTotal.toFixed(2) }}€
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
</style>
