<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  task: {
    type: Object,
    default: null,
  },
})

const emit = defineEmits(['close', 'save'])

const form = ref({
  id: null,
  title: '',
  type: 'Work',
  start: '',
  end: '',
  rating: '',
  description: '',
  repeat: 'None',
})

onMounted(() => {
  if (props.task) {
    form.value = { repeat: 'None', ...props.task }
    if (form.value.rating === 0) form.value.rating = ''
    if (!form.value.repeat) form.value.repeat = 'None'
  }
})

const saveTask = () => {
  const toSave = { ...form.value }
  if (toSave.rating === '' || toSave.rating === null) {
    delete toSave.rating
  }
  emit('save', toSave)
  emit('close')
}
</script>

<template>
  <div
    class="fixed inset-0 bg-black/60 backdrop-blur-md overflow-y-auto h-full w-full flex items-start justify-center pt-10 pb-10 z-50"
    id="my-modal"
    @click.self="$emit('close')"
  >
    <div
      class="relative mx-auto p-8 w-full max-w-md glass-effect rounded-2xl border border-blue-400/30 shadow-2xl"
    >
      <div
        class="absolute inset-0 bg-gradient-to-br from-blue-600/10 to-slate-700/10 rounded-2xl"
      ></div>

      <div class="relative z-10">
        <div class="text-center mb-6">
          <h3 class="text-2xl font-bold text-slate-100">
            {{ form.id ? 'Edit Task' : 'Create New Task' }}
          </h3>
          <div
            class="w-16 h-1 bg-gradient-to-r from-blue-400 to-blue-600 mx-auto mt-3 rounded-full"
          ></div>
        </div>

        <form @submit.prevent="saveTask" class="space-y-6">
          <div>
            <label
              for="title"
              class="block text-sm font-medium text-slate-300 mb-2"
            >
              Task Title
            </label>
            <input
              type="text"
              v-model="form.title"
              id="title"
              required
              placeholder="Enter task title..."
              class="w-full px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
            />
          </div>

          <div>
            <label
              for="description"
              class="block text-sm font-medium text-slate-300 mb-2"
            >
              Description
            </label>
            <textarea
              v-model="form.description"
              id="description"
              rows="3"
              placeholder="Add task description..."
              class="w-full px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300 resize-none"
            ></textarea>
          </div>

          <div class="grid grid-cols-2 gap-4">
            <div>
              <label
                for="type"
                class="block text-sm font-medium text-slate-300 mb-2"
              >
                Category
              </label>
              <select
                v-model="form.type"
                id="type"
                class="w-full px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
              >
                <option value="Work">💼 Work</option>
                <option value="Rest">😴 Rest</option>
                <option value="Study">📚 Study</option>
                <option value="Personal">🏠 Personal</option>
                <option value="Prayer">🙏 Prayer</option>
                <option value="Activity">🏃 Activity</option>
              </select>
            </div>

            <div>
              <label
                for="repeat"
                class="block text-sm font-medium text-slate-300 mb-2"
              >
                Repeat
              </label>
              <select
                v-model="form.repeat"
                id="repeat"
                class="w-full px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
              >
                <option value="None">None</option>
                <option value="Every Day">Every Day</option>
                <option value="Weekdays Only">Weekdays</option>
                <option value="Weekends Only">Weekends</option>
              </select>
            </div>
          </div>

          <div class="grid grid-cols-2 gap-4">
            <div>
              <label
                for="start"
                class="block text-sm font-medium text-slate-300 mb-2"
              >
                Start Time
              </label>
              <input
                type="time"
                v-model="form.start"
                id="start"
                required
                class="w-full px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
              />
            </div>

            <div>
              <label
                for="end"
                class="block text-sm font-medium text-slate-300 mb-2"
              >
                End Time
              </label>
              <input
                type="time"
                v-model="form.end"
                id="end"
                required
                class="w-full px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
              />
            </div>
          </div>

          <div>
            <label
              for="rating"
              class="block text-sm font-medium text-slate-300 mb-2"
            >
              Priority Rating (1-5)
            </label>
            <input
              type="number"
              v-model="form.rating"
              id="rating"
              min="1"
              max="5"
              placeholder="Optional priority..."
              class="w-full px-4 py-3 bg-slate-700/50 border border-blue-400/30 rounded-lg text-slate-100 placeholder-slate-400 focus:border-blue-400 focus:ring-2 focus:ring-blue-400/20 transition-all duration-300"
            />
            <p class="text-xs text-slate-400 mt-1">
              Optional - Rate task importance
            </p>
          </div>

          <div class="flex gap-3 pt-4">
            <button
              type="submit"
              class="flex-1 px-6 py-3 bg-gradient-to-r from-blue-500 to-blue-600 text-white font-medium rounded-lg hover:from-blue-600 hover:to-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500/50 transition-all duration-300 shadow-lg shadow-blue-500/25"
            >
              {{ form.id ? 'Update Task' : 'Create Task' }}
            </button>
            <button
              @click="$emit('close')"
              type="button"
              class="flex-1 px-6 py-3 bg-gradient-to-r from-slate-600 to-slate-700 text-slate-200 font-medium rounded-lg hover:from-slate-700 hover:to-slate-800 focus:outline-none focus:ring-2 focus:ring-slate-500/50 transition-all duration-300"
            >
              Cancel
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped>
.glass-effect {
  background: rgba(15, 24, 40, 0.9);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(59, 130, 246, 0.3);
}

input[type='time']::-webkit-calendar-picker-indicator {
  filter: invert(1);
  cursor: pointer;
}

select option {
  background: #1e293b;
  color: #f1f5f9;
}
</style>
