<script setup>
import { ref, watch, computed } from 'vue'
import { useMoods } from '../composables/useMoods'

const props = defineProps({
  date: {
    type: String,
    required: true,
  },
  isEditable: {
    type: Boolean,
    default: true,
  },
})

const { getMood, setMood: saveMood, moods } = useMoods()
const selectedMood = ref(getMood(props.date) || null)

const moodOptions = [
  { value: 1, emoji: '😞', label: 'Very Bad' },
  { value: 2, emoji: '😐', label: 'Not Great' },
  { value: 3, emoji: '🙂', label: 'Okay' },
  { value: 4, emoji: '😄', label: 'Good' },
  { value: 5, emoji: '😁', label: 'Excellent' },
]

const getMoodLabel = (value) => {
  const mood = moodOptions.find((m) => m.value === value)
  return mood ? mood.label : ''
}

const selectedEmoji = computed(() => {
  if (!selectedMood.value) return null
  const mood = moodOptions.find((m) => m.value === selectedMood.value)
  return mood ? mood.emoji : null
})

const setMood = (mood) => {
  if (!props.isEditable) return
  selectedMood.value = mood
  saveMood(props.date, mood)
}

watch(
  [() => props.date, () => moods.value[props.date]],
  ([newDate]) => {
    selectedMood.value = getMood(newDate) || null
  },
  { immediate: true }
)
</script>

<template>
  <div
    class="flex items-center space-x-4 glass-effect p-6 rounded-xl border border-blue-400/20"
  >
    <div class="flex items-center gap-3">
      <div class="w-2 h-2 bg-blue-400 rounded-full animate-pulse"></div>
      <span class="text-slate-200 font-medium">{{
        isEditable ? "How's your day going?" : "The day's mood was:"
      }}</span>
    </div>

    <template v-if="isEditable">
      <div class="flex items-center gap-2">
        <button
          v-for="mood in moodOptions"
          :key="mood.value"
          @click="setMood(mood.value)"
          :class="[
            'p-3 rounded-xl cursor-pointer transition-all duration-300 hover:scale-110 border-2',
            selectedMood === mood.value
              ? 'bg-gradient-to-r from-blue-500 to-blue-600 border-blue-400 shadow-lg shadow-blue-500/30'
              : 'border-slate-600/50 hover:border-blue-400/50 hover:bg-blue-600/10',
          ]"
          :title="getMoodLabel(mood.value)"
        >
          <span class="text-2xl block">{{ mood.emoji }}</span>
        </button>
      </div>
    </template>

    <div v-else class="flex items-center gap-3">
      <div
        class="p-3 bg-gradient-to-r from-slate-600 to-slate-700 rounded-xl border border-slate-500"
      >
        <span class="text-3xl">{{ selectedEmoji || '❓' }}</span>
      </div>
      <span class="text-slate-300 font-medium">{{
        selectedEmoji ? getMoodLabel(selectedMood) : 'Not set'
      }}</span>
    </div>
  </div>
</template>
