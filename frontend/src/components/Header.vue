<script setup>
import { ref } from 'vue'
import { useAuth } from '../composables/useAuth'
import {
  CalendarDaysIcon,
  CalendarIcon,
  Squares2X2Icon,
  ClockIcon,
  UserIcon,
  ArrowRightStartOnRectangleIcon,
  ArrowLeftStartOnRectangleIcon,
} from '@heroicons/vue/24/outline'

const props = defineProps({
  currentView: String,
})
const emit = defineEmits([
  'setView',
  'open-auth-modal',
  'logout',
  'go-to-landing',
])

const isMenuOpen = ref(false)

function handleNavItemClick(view) {
  emit('setView', view)
  isMenuOpen.value = false
}

const { user, isAuthenticated } = useAuth()

const navItems = [
  { view: 'DayView', label: 'Day', icon: CalendarDaysIcon },
  { view: 'WeekView', label: 'Week', icon: CalendarIcon },
  { view: 'MonthView', label: 'Month', icon: Squares2X2Icon },
  { view: 'TimelineView', label: 'Timeline', icon: ClockIcon },
]
</script>

<template>
  <header
    class="glass-effect border-b border-blue-400/20 p-4 w-full relative overflow-hidden bg-gradient-to-r from-slate-900 via-blue-900 to-slate-900"
  >
    <div
      class="absolute inset-0 bg-gradient-to-r from-blue-600/5 to-slate-700/5"
    ></div>

    <div
      class="container mx-auto flex justify-between items-center w-full relative z-10"
    >
      <div class="flex items-center space-x-3">
        <button
          @click="$emit('go-to-landing')"
          class="flex items-center space-x-3 group"
        >
          <div
            class="w-8 h-8 bg-gradient-to-r from-blue-600 to-blue-500 rounded-lg flex items-center justify-center"
          >
            <div class="w-4 h-4 bg-white rounded-sm"></div>
          </div>
          <h1
            class="text-2xl font-bold text-white group-hover:text-blue-200 transition-all duration-300"
          >
            FlowBit
          </h1>
        </button>
      </div>

      <div class="hidden md:flex items-center space-x-2">
        <button
          v-for="item in navItems"
          :key="item.view"
          @click="handleNavItemClick(item.view)"
          :class="[
            'px-4 py-2 rounded-lg text-base font-medium text-white flex items-center gap-2 transition-all duration-300 relative group',
            currentView === item.view
              ? 'bg-gradient-to-r from-blue-600 to-blue-700 text-white shadow-lg shadow-blue-500/25'
              : 'hover:bg-gradient-to-r hover:from-blue-600/30 hover:to-blue-700/30 hover:text-blue-200',
          ]"
          class="cursor-pointer"
        >
          <component :is="item.icon" class="w-5 h-5 relative z-10" />
          <span class="relative z-10">{{ item.label }}</span>
        </button>

        <div class="ml-6 flex items-center gap-3">
          <template v-if="isAuthenticated">
            <div
              class="flex items-center gap-2 px-3 py-2 bg-gradient-to-r from-slate-700/50 to-slate-600/50 rounded-lg border border-slate-500/30"
            >
              <UserIcon class="w-5 h-5 text-blue-300" />
              <span class="text-white font-medium">{{ user.username }}</span>
            </div>
            <button
              @click="$emit('logout')"
              class="px-4 py-2 bg-gradient-to-r from-red-500 to-red-600 text-white rounded-lg hover:from-red-600 hover:to-red-700 flex items-center gap-2 transition-all duration-300 shadow-lg shadow-red-500/25"
            >
              <ArrowRightStartOnRectangleIcon class="w-5 h-5" />
              <span class="font-medium">Logout</span>
            </button>
          </template>
          <template v-else>
            <button
              @click="$emit('open-auth-modal')"
              class="px-4 py-2 bg-gradient-to-r from-blue-500 to-blue-600 text-white rounded-lg hover:from-blue-600 hover:to-blue-700 flex items-center gap-2 transition-all duration-300 shadow-lg shadow-blue-500/25 glow-effect-hover"
            >
              <ArrowLeftStartOnRectangleIcon class="w-5 h-5" />
              <span class="font-medium">Login</span>
            </button>
          </template>
        </div>
      </div>

      <div class="md:hidden">
        <button
          @click="isMenuOpen = !isMenuOpen"
          class="cursor-pointer p-3 rounded-lg glass-effect border border-blue-400/30 hover:bg-blue-600/20 transition-all duration-300"
        >
          <svg
            class="w-6 h-6 text-white"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 6h16M4 12h16m-7 6h7"
            ></path>
          </svg>
        </button>
      </div>
    </div>

    <transition
      enter-active-class="transition ease-out duration-200"
      enter-from-class="transform opacity-0 scale-95"
      enter-to-class="transform opacity-100 scale-100"
      leave-active-class="transition ease-in duration-75"
      leave-from-class="transform opacity-100 scale-100"
      leave-to-class="transform opacity-0 scale-95"
    >
      <div
        v-if="isMenuOpen"
        class="md:hidden mt-4 origin-top-right glass-effect rounded-xl shadow-2xl p-6 border border-blue-400/30"
      >
        <nav class="flex flex-col space-y-3">
          <button
            v-for="item in navItems"
            :key="item.view"
            @click="handleNavItemClick(item.view)"
            :class="[
              'px-4 py-3 rounded-lg text-left cursor-pointer text-base font-medium text-slate-200 flex items-center gap-3 transition-all duration-300',
              currentView === item.view
                ? 'bg-gradient-to-r from-blue-600 to-blue-700 text-white shadow-lg shadow-blue-500/25'
                : 'hover:bg-gradient-to-r hover:from-blue-600/30 hover:to-blue-700/30 hover:text-white',
            ]"
          >
            <component :is="item.icon" class="w-5 h-5" />
            {{ item.label }}
          </button>

          <template v-if="isAuthenticated">
            <div
              class="flex items-center gap-3 px-4 py-3 bg-gradient-to-r from-slate-700/50 to-slate-600/50 rounded-lg border border-slate-500/30"
            >
              <UserIcon class="w-5 h-5 text-blue-300" />
              <span class="text-slate-200 font-medium">{{
                user.username
              }}</span>
            </div>
            <button
              @click="
                () => {
                  $emit('logout')
                  isMenuOpen = false
                }
              "
              class="px-4 py-3 bg-gradient-to-r from-red-500 to-red-600 text-white rounded-lg hover:from-red-600 hover:to-red-700 flex items-center gap-3 transition-all duration-300 shadow-lg shadow-red-500/25"
            >
              <ArrowRightStartOnRectangleIcon class="w-5 h-5" />
              <span class="font-medium">Logout</span>
            </button>
          </template>
          <template v-else>
            <button
              @click="
                () => {
                  $emit('open-auth-modal')
                  isMenuOpen = false
                }
              "
              class="px-4 py-3 bg-gradient-to-r from-blue-500 to-blue-600 text-white rounded-lg hover:from-blue-600 hover:to-blue-700 flex items-center gap-3 transition-all duration-300 shadow-lg shadow-blue-500/25"
            >
              <ArrowLeftStartOnRectangleIcon class="w-5 h-5" />
              <span class="font-medium">Login</span>
            </button>
          </template>
        </nav>
      </div>
    </transition>
  </header>
</template>

<style scoped>
@keyframes slide-in {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0);
  }
}

.animate-slide-in {
  animation: slide-in 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.sidebar-fade-enter-active,
.sidebar-fade-leave-active {
  transition: opacity 0.2s;
}

.sidebar-fade-enter-from,
.sidebar-fade-leave-to {
  opacity: 0;
}

.glass-effect {
  background: rgba(15, 24, 40, 0.8);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(59, 130, 246, 0.2);
}
</style>
