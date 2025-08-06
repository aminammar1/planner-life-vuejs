<script setup>
import { ref, watch } from 'vue'
import { useAuth } from '../composables/useAuth'
import {
  UserIcon,
  LockClosedIcon,
  XMarkIcon,
  EyeIcon,
  EyeSlashIcon,
} from '@heroicons/vue/24/outline'

const tab = ref('login')
const username = ref('')
const password = ref('')
const error = ref('')
const captchaQuestion = ref('')
const captchaAnswer = ref('')
const userCaptchaInput = ref('')
const showPassword = ref(false)

const { login, register } = useAuth()

const activeTabClass =
  'flex-1 py-2 border-b-2 border-blue-600 text-blue-600 font-semibold'
const inactiveTabClass =
  'flex-1 py-2 border-b-2 border-transparent text-gray-500'

const emit = defineEmits(['close', 'auth-success'])

function generateCaptcha() {
  const a = Math.floor(Math.random() * 10) + 1
  const b = Math.floor(Math.random() * 10) + 1
  const ops = ['+', '-']
  const op = ops[Math.floor(Math.random() * ops.length)]
  let left = a,
    right = b
  if (op === '-' && a < b) {
    left = b
    right = a
  }
  captchaQuestion.value = `${left} ${op} ${right}`
  captchaAnswer.value = op === '+' ? left + right : left - right
  userCaptchaInput.value = ''
}

watch(tab, (newTab) => {
  if (newTab === 'register') generateCaptcha()
})

// Generate on mount if register tab is default
if (tab.value === 'register') generateCaptcha()

const handleLogin = async () => {
  error.value = ''
  try {
    await login(username.value, password.value)
    username.value = ''
    password.value = ''
    error.value = ''
    emit('close')
    emit('auth-success')
  } catch (e) {
    error.value = e.response?.data?.message || 'Login failed.'
  }
}

const handleRegister = async () => {
  error.value = ''
  try {
    await register(
      username.value,
      password.value,
      captchaQuestion.value,
      userCaptchaInput.value
    )
    username.value = ''
    password.value = ''
    error.value = ''
    emit('close')
    emit('auth-success')
  } catch (e) {
    error.value = e.response?.data?.error || 'Registration failed.'
    generateCaptcha() // Regenerate on failure
  }
}
</script>

<template>
  <div
    class="fixed inset-0 bg-black/70 backdrop-blur-md overflow-y-auto h-full w-full flex items-center justify-center pt-10 pb-10 z-50"
  >
    <div
      class="glass-effect rounded-2xl shadow-2xl w-full max-w-md p-8 relative animate-fade-in border border-blue-400/30"
    >
      <div
        class="absolute inset-0 bg-gradient-to-br from-blue-600/10 to-slate-700/10 rounded-2xl"
      ></div>

      <button
        class="absolute top-4 right-4 text-slate-400 hover:text-slate-200 transition-colors duration-300 p-2 hover:bg-slate-600/30 rounded-lg"
        @click="$emit('close')"
        aria-label="Close"
      >
        <XMarkIcon class="w-6 h-6 cursor-pointer" />
      </button>

      <div class="relative z-10">
        <div class="text-center mb-8">
          <h2
            class="text-3xl font-bold bg-gradient-to-r from-blue-300 to-white bg-clip-text text-transparent"
          >
            Welcome Back
          </h2>
          <p class="text-slate-400 mt-2">Sign in to your productivity hub</p>
        </div>

        <div class="flex mb-8 bg-slate-700/30 rounded-xl p-1">
          <button
            :class="[
              'flex-1 py-3 px-4 rounded-lg font-medium transition-all duration-300',
              tab === 'login'
                ? 'bg-gradient-to-r from-blue-500 to-blue-600 text-white shadow-lg'
                : 'text-slate-300 hover:text-white hover:bg-slate-600/30',
            ]"
            @click="tab = 'login'"
          >
            Login
          </button>
          <button
            :class="[
              'flex-1 py-3 px-4 rounded-lg font-medium transition-all duration-300',
              tab === 'register'
                ? 'bg-gradient-to-r from-blue-500 to-blue-600 text-white shadow-lg'
                : 'text-slate-300 hover:text-white hover:bg-slate-600/30',
            ]"
            @click="tab = 'register'"
          >
            Register
          </button>
        </div>

        <form
          @submit.prevent="tab === 'login' ? handleLogin() : handleRegister()"
          autocomplete="off"
          class="space-y-6"
        >
          <div class="relative">
            <label class="block text-slate-300 mb-2 font-medium"
              >Username</label
            >
            <div class="relative">
              <span
                class="absolute left-4 top-1/2 -translate-y-1/2 text-blue-300"
              >
                <UserIcon class="w-5 h-5" />
              </span>
              <input
                v-model="username"
                class="w-full bg-slate-700/50 border border-blue-400/30 rounded-xl px-12 py-4 text-slate-100 placeholder-slate-400 focus:ring-2 focus:ring-blue-400/50 focus:border-blue-400 transition-all duration-300"
                required
                placeholder="Enter your username"
              />
            </div>
          </div>

          <div class="relative">
            <label class="block text-slate-300 mb-2 font-medium"
              >Password</label
            >
            <div class="relative">
              <span
                class="absolute left-4 top-1/2 -translate-y-1/2 text-blue-300"
              >
                <LockClosedIcon class="w-5 h-5" />
              </span>
              <input
                v-model="password"
                :type="showPassword ? 'text' : 'password'"
                class="w-full bg-slate-700/50 border border-blue-400/30 rounded-xl px-12 py-4 pr-14 text-slate-100 placeholder-slate-400 focus:ring-2 focus:ring-blue-400/50 focus:border-blue-400 transition-all duration-300"
                required
                placeholder="Enter your password"
              />
              <button
                type="button"
                @click="showPassword = !showPassword"
                class="absolute right-4 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-200 focus:outline-none transition-colors duration-300"
              >
                <component
                  :is="showPassword ? EyeSlashIcon : EyeIcon"
                  class="w-5 h-5"
                />
              </button>
            </div>
          </div>

          <div v-if="tab === 'register'" class="relative">
            <label class="block text-slate-300 mb-2 font-medium">
              Security Check: What is {{ captchaQuestion }}?
            </label>
            <input
              v-model="userCaptchaInput"
              class="w-full bg-slate-700/50 border border-blue-400/30 rounded-xl px-4 py-4 text-slate-100 placeholder-slate-400 focus:ring-2 focus:ring-blue-400/50 focus:border-blue-400 transition-all duration-300"
              required
              autocomplete="off"
              inputmode="numeric"
              pattern="[0-9]*"
              placeholder="Enter the answer"
            />
          </div>

          <div
            v-if="error"
            class="bg-red-500/10 border border-red-400/30 rounded-xl p-4 text-red-300 text-sm text-center"
          >
            {{ error }}
          </div>

          <button
            type="submit"
            class="w-full bg-gradient-to-r from-blue-500 to-blue-600 text-white py-4 rounded-xl font-semibold hover:from-blue-600 hover:to-blue-700 transition-all duration-300 shadow-lg shadow-blue-500/25 hover:shadow-blue-500/40"
          >
            <span v-if="tab === 'login'">Sign In</span>
            <span v-else>Create Account</span>
          </button>
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

.animate-fade-in {
  animation: fade-in 0.3s ease-out;
}

@keyframes fade-in {
  from {
    opacity: 0;
    transform: scale(0.95) translateY(-10px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}
</style>

<style scoped>
@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
.animate-fade-in {
  animation: fade-in 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}
</style>
