<script setup>
import { ref } from 'vue'
import Header from './components/Header.vue'
import DayView from './views/DayView.vue'
import WeekView from './views/WeekView.vue'
import MonthView from './views/MonthView.vue'
import TimelineView from './views/TimelineView.vue'
import LandingPage from './views/LandingPage.vue'
import LoginRegisterModal from './components/LoginRegisterModal.vue'
import Toast from './components/Toast.vue'
import { toYYYYMMDD } from './utils/date'
import { useTasks } from './composables/useTasks'
import { useExpenses } from './composables/useExpenses'
import { useMoods } from './composables/useMoods'
import { useAuth } from './composables/useAuth'

const showLandingPage = ref(true)
const currentView = ref('DayView')
const currentDate = ref(toYYYYMMDD(new Date()))
const showAuthModal = ref(false)

const views = {
  DayView,
  WeekView,
  MonthView,
  TimelineView,
}

const setView = (view) => {
  currentView.value = view
}

const changeDate = (newDate) => {
  currentDate.value = newDate
}

const setViewWithDate = ({ view, date }) => {
  currentView.value = view
  currentDate.value = date
}

const enterApp = () => {
  showLandingPage.value = false
}

const goToLanding = () => {
  showLandingPage.value = true
}

const { fetchTasks } = useTasks()
const { fetchExpenses } = useExpenses()
const { fetchMoods } = useMoods()
const { logout, isAuthenticated } = useAuth()

if (isAuthenticated.value) {
  showLandingPage.value = false
}

function refetchAllData() {
  fetchTasks()
  fetchExpenses()
  fetchMoods()
}

function handleLogout() {
  logout()
  refetchAllData()
}

function handleAuthSuccess() {
  refetchAllData()
  showAuthModal.value = false
  showLandingPage.value = false
}
</script>

<template>
  <div
    id="app"
    class="min-h-screen w-full text-slate-100 relative overflow-hidden"
  >
    <div
      class="absolute inset-0 bg-gradient-to-br from-slate-900 via-blue-900 to-slate-800"
    ></div>
    <div
      class="absolute inset-0 bg-[radial-gradient(circle_at_30%_20%,rgba(59,130,246,0.1)_0%,transparent_50%)]"
    ></div>
    <div
      class="absolute inset-0 bg-[radial-gradient(circle_at_70%_80%,rgba(37,99,235,0.1)_0%,transparent_50%)]"
    ></div>

    <div class="relative z-10" v-if="showLandingPage">
      <LandingPage
        @open-auth-modal="showAuthModal = true"
        @enter-app="enterApp"
        @go-to-landing="goToLanding"
      />
    </div>

    <div class="relative z-10" v-else>
      <Header
        :currentView="currentView"
        @setView="setView"
        @open-auth-modal="showAuthModal = true"
        @logout="handleLogout"
        @go-to-landing="goToLanding"
      />
      <main class="p-2 sm:p-4 w-full max-w-auto mx-auto">
        <component
          :is="views[currentView]"
          :current-date="currentDate"
          @change-date="changeDate"
          v-if="currentView === 'DayView' || currentView === 'TimelineView'"
        />
        <component
          :is="views[currentView]"
          :current-date="currentDate"
          @change-date="changeDate"
          @set-view-with-date="setViewWithDate"
          v-else
        />
      </main>
    </div>

    <LoginRegisterModal
      v-if="showAuthModal"
      @close="showAuthModal = false"
      @auth-success="handleAuthSuccess"
    />
    <Toast />
  </div>
</template>

<style></style>
