<script setup>
import { ref, computed, watch } from 'vue'
import { useExpenses } from '../composables/useExpenses'

const props = defineProps({
  date: { type: String, required: true }, // format: YYYY-MM-DD
})

const month = computed(() => props.date.slice(0, 7))

const {
  getExpensesByDate,
  getExpensesByMonth,
  addExpense,
  editExpense,
  deleteExpense: removeExpense,
  getTotalByDate,
  getTotalByMonth,
  getDailyBudget,
  setDailyBudget,
  getMonthlyBudget,
  setMonthlyBudget,
} = useExpenses()

const form = ref({ amount: '', description: '', category: '' })
const editingId = ref(null)

const expensesToday = computed(() => getExpensesByDate(props.date))
const totalToday = computed(() => getTotalByDate(props.date))
const totalMonth = computed(() => getTotalByMonth(month.value))

const dailyBudget = ref(getDailyBudget(props.date))
const monthlyBudget = ref(getMonthlyBudget())
const dailyBudgetInput = ref(dailyBudget.value)
const monthlyBudgetInput = ref(monthlyBudget.value)

const collapsed = ref(false)

function toggleCollapse() {
  collapsed.value = !collapsed.value
}

function onSubmit() {
  if (editingId.value) {
    editExpense(editingId.value, {
      amount: parseFloat(form.value.amount),
      description: form.value.description,
      category: form.value.category,
    })
    editingId.value = null
  } else {
    addExpense({
      date: props.date,
      amount: parseFloat(form.value.amount),
      description: form.value.description,
      category: form.value.category,
    })
  }
  form.value = { amount: '', description: '', category: '' }
}

function startEdit(expense) {
  editingId.value = expense._id
  form.value = {
    amount: expense.amount,
    description: expense.description || expense.note,
    category: expense.category,
  }
}

function cancelEdit() {
  editingId.value = null
  form.value = { amount: '', description: '', category: '' }
}

function deleteExpense(id) {
  removeExpense(id)
  if (editingId.value === id) {
    cancelEdit()
  }
}

function updateDailyBudget() {
  setDailyBudget(dailyBudgetInput.value, props.date)
  dailyBudget.value = getDailyBudget(props.date)
}
function updateMonthlyBudget() {
  setMonthlyBudget(monthlyBudgetInput.value)
  monthlyBudget.value = getMonthlyBudget()
}

// Keep budgets in sync if changed elsewhere
watch(
  () => getDailyBudget(props.date),
  (val) => {
    dailyBudget.value = val
    dailyBudgetInput.value = val
  }
)
watch(
  () => getMonthlyBudget(),
  (val) => {
    monthlyBudget.value = val
    monthlyBudgetInput.value = val
  }
)
</script>

<template>
  <div
    class="expense-tracker glass-effect rounded-xl border border-blue-400/20 shadow-lg p-6 space-y-6 mt-6 relative overflow-hidden"
  >
    <div
      class="absolute inset-0 bg-gradient-to-br from-blue-600/5 to-transparent"
    ></div>

    <div
      class="flex flex-col sm:flex-row items-start sm:items-center justify-between cursor-pointer select-none hover:bg-blue-600/10 rounded-lg p-3 transition-all duration-300 relative z-10"
      @click="toggleCollapse"
    >
      <div class="flex items-center gap-3 mb-2 sm:mb-0">
        <div
          class="w-3 h-3 bg-gradient-to-r from-blue-400 to-blue-500 rounded-full animate-pulse"
        ></div>
        <h2 class="text-xl font-bold text-slate-100">Expense Tracker</h2>
      </div>

      <div
        class="flex flex-col sm:flex-row items-start sm:items-center space-y-3 sm:space-y-0 sm:space-x-4 w-full sm:w-auto"
      >
        <div
          class="flex flex-col sm:flex-row gap-3 w-full sm:w-auto"
          v-if="!collapsed"
        >
          <label
            class="flex flex-col sm:flex-row sm:items-center gap-2 text-sm w-full sm:w-auto"
            @click.stop
          >
            <span class="text-slate-300 font-medium">Daily Budget:</span>
            <input
              type="number"
              v-model.number="dailyBudgetInput"
              @change="updateDailyBudget"
              min="0"
              class="w-full sm:w-24 px-3 py-2 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
            />
          </label>
          <label
            class="flex flex-col sm:flex-row sm:items-center gap-2 text-sm w-full sm:w-auto"
            @click.stop
          >
            <span class="text-slate-300 font-medium">Monthly Budget:</span>
            <input
              type="number"
              v-model.number="monthlyBudgetInput"
              @change="updateMonthlyBudget"
              min="0"
              class="w-full sm:w-28 px-3 py-2 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
            />
          </label>
        </div>

        <div
          class="p-2 rounded-lg hover:bg-blue-600/20 transition-all duration-300"
        >
          <svg
            :class="[
              'w-6 h-6 transition-transform duration-300 text-blue-300',
              collapsed ? 'rotate-180' : 'rotate-0',
            ]"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M19 9l-7 7-7-7"
            />
          </svg>
        </div>
      </div>
    </div>

    <transition name="collapse-smooth">
      <div v-show="!collapsed" class="collapse-content relative z-10">
        <div
          class="grid grid-cols-1 md:grid-cols-2 gap-6 p-4 bg-gradient-to-r from-slate-800/30 to-slate-700/30 rounded-xl border border-slate-600/30"
        >
          <div class="space-y-3">
            <div class="flex justify-between items-center">
              <span class="font-medium text-slate-300">Spent Today:</span>
              <span class="text-red-400 font-bold text-lg"
                >{{ totalToday.toFixed(2) }} €</span
              >
            </div>
            <div class="flex justify-between items-center">
              <span class="font-medium text-slate-300">Remaining Today:</span>
              <span
                :class="[
                  'font-bold text-lg',
                  dailyBudget - totalToday < 0
                    ? 'text-red-400'
                    : 'text-green-400',
                ]"
              >
                {{ (dailyBudget - totalToday).toFixed(2) }} €
              </span>
            </div>
          </div>

          <div class="space-y-3 md:border-l md:border-slate-600/50 md:pl-6">
            <div class="flex justify-between items-center">
              <span class="font-medium text-slate-300">Spent This Month:</span>
              <span class="text-red-400 font-bold text-lg"
                >{{ totalMonth.toFixed(2) }} €</span
              >
            </div>
            <div class="flex justify-between items-center">
              <span class="font-medium text-slate-300"
                >Remaining This Month:</span
              >
              <span
                :class="[
                  'font-bold text-lg',
                  monthlyBudget - totalMonth < 0
                    ? 'text-red-400'
                    : 'text-green-400',
                ]"
              >
                {{ (monthlyBudget - totalMonth).toFixed(2) }} €
              </span>
            </div>
          </div>
        </div>

        <form
          @submit.prevent="onSubmit"
          class="flex flex-col sm:flex-row sm:items-end gap-3 border-t border-slate-600/30 pt-6 mt-6"
        >
          <input
            v-model="form.amount"
            type="number"
            step="0.01"
            min="0"
            placeholder="Amount (€)"
            required
            class="px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg w-full sm:w-32 text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
          />
          <input
            v-model="form.description"
            type="text"
            placeholder="Description"
            required
            class="px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg w-full flex-1 text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
          />
          <input
            v-model="form.category"
            type="text"
            placeholder="Category"
            class="px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg w-full sm:w-36 text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
          />
          <div class="flex gap-3 w-full sm:w-auto">
            <button
              type="submit"
              class="bg-gradient-to-r from-blue-500 to-blue-600 text-white px-6 py-3 rounded-lg hover:from-blue-600 hover:to-blue-700 font-medium w-full sm:w-auto transition-all duration-300 shadow-lg shadow-blue-500/25"
            >
              {{ editingId ? 'Update' : 'Add' }}
            </button>
            <button
              v-if="editingId"
              type="button"
              @click="cancelEdit"
              class="bg-gradient-to-r from-slate-600 to-slate-700 text-slate-200 px-6 py-3 rounded-lg hover:from-slate-700 hover:to-slate-800 font-medium w-full sm:w-auto transition-all duration-300"
            >
              Cancel
            </button>
          </div>
        </form>

        <div class="space-y-3">
          <div
            v-for="expense in expensesToday"
            :key="expense._id"
            class="flex flex-col sm:flex-row justify-between items-start sm:items-center p-4 glass-effect rounded-lg border border-slate-600/30 hover:border-blue-400/30 transition-all duration-300 group"
          >
            <div class="flex-1">
              <div class="flex items-center gap-3 mb-1">
                <span class="font-bold text-red-400 text-lg"
                  >{{ expense.amount.toFixed(2) }} €</span
                >
                <span class="text-slate-400">•</span>
                <span class="text-slate-200 font-medium">{{
                  expense.description
                }}</span>
              </div>
              <div
                v-if="expense.category"
                class="text-sm text-blue-300 font-medium"
              >
                📂 {{ expense.category }}
              </div>
            </div>

            <div class="flex gap-2 mt-2 sm:mt-0">
              <button
                @click="startEdit(expense)"
                class="text-blue-300 hover:text-blue-100 p-2 rounded-lg hover:bg-blue-600/20 transition-all duration-300"
                title="Edit expense"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                >
                  <path
                    d="M13.586 3.586a2 2 0 112.828 2.828l-8 8a2 2 0 01-1.414.586H4a2 2 0 01-2-2v-2.172a2 2 0 01.586-1.414l8-8zM12 5l-8 8v1h1l8-8-1-1z"
                  />
                </svg>
              </button>
              <button
                @click="deleteExpense(expense._id)"
                class="text-slate-400 hover:text-red-400 p-2 rounded-lg hover:bg-red-600/20 transition-all duration-300"
                title="Delete expense"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5"
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

          <div
            v-if="expensesToday.length === 0"
            class="text-slate-400 text-center py-8 italic"
          >
            No expenses for today.
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<style scoped>
.glass-effect {
  background: rgba(26, 32, 44, 0.8);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(59, 130, 246, 0.2);
}

.collapse-smooth-enter-active,
.collapse-smooth-leave-active {
  transition: max-height 0.4s cubic-bezier(0.4, 0, 0.2, 1),
    opacity 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.collapse-smooth-enter-from,
.collapse-smooth-leave-to {
  max-height: 0;
  opacity: 0;
}

.collapse-smooth-enter-to,
.collapse-smooth-leave-from {
  max-height: 2000px;
  opacity: 1;
}

.collapse-content {
  overflow: hidden;
}
</style>
