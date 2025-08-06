<script setup>
import { useToast } from '../composables/useToast'
const { toastMessage, toastVisible } = useToast()
</script>

<template>
  <transition name="fade">
    <div
      v-if="toastVisible"
      class="fixed top-8 left-1/2 transform -translate-x-1/2 glass-effect px-8 py-4 rounded-xl shadow-2xl z-50 border border-green-400/30"
    >
      <div class="flex items-center gap-3">
        <div class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
        <span class="text-green-200 font-medium">{{ toastMessage }}</span>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.glass-effect {
  background: rgba(15, 24, 40, 0.9);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(34, 197, 94, 0.3);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.fade-enter-from {
  opacity: 0;
  transform: translateX(-50%) translateY(-20px) scale(0.95);
}

.fade-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(-20px) scale(0.95);
}
</style>
