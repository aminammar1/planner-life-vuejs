<template>
  <div
    class="min-h-screen bg-gradient-to-br from-slate-900 via-blue-900 to-slate-800 relative overflow-hidden"
  >
    <div
      class="absolute inset-0 bg-[radial-gradient(circle_at_30%_20%,rgba(59,130,246,0.1)_0%,transparent_50%)]"
    ></div>
    <div
      class="absolute inset-0 bg-[radial-gradient(circle_at_70%_80%,rgba(37,99,235,0.1)_0%,transparent_50%)]"
    ></div>

    <div class="relative z-10 min-h-screen">
      <header
        class="fixed top-0 left-0 right-0 z-50 bg-slate-900/90 backdrop-blur-md border-b border-blue-400/20"
      >
        <div
          class="max-w-7xl mx-auto px-6 py-4 flex items-center justify-between"
        >
          <button
            @click="$emit('go-to-landing')"
            class="flex items-center space-x-3 group"
          >
            <div
              class="w-8 h-8 bg-gradient-to-r from-blue-600 to-blue-500 rounded-lg flex items-center justify-center"
            >
              <div class="w-4 h-4 bg-white rounded-sm"></div>
            </div>
            <span
              class="text-2xl font-bold text-white group-hover:text-blue-200 transition-colors"
            >
              FlowBit
            </span>
          </button>

          <nav class="hidden md:flex items-center space-x-8">
            <a
              href="#features"
              class="text-blue-200 hover:text-white transition-colors"
              >Features</a
            >
            <a
              href="#about"
              class="text-blue-200 hover:text-white transition-colors"
              >About</a
            >
            <button
              @click="$emit('open-auth-modal')"
              class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors"
            >
              Get Started
            </button>
          </nav>

          <button
            class="md:hidden p-2 text-blue-200 hover:text-white transition-colors"
          >
            <svg
              class="w-6 h-6"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4 6h16M4 12h16M4 18h16"
              ></path>
            </svg>
          </button>
        </div>
      </header>

      <main class="pt-20">
        <section class="max-w-7xl mx-auto px-6 py-20">
          <div class="grid lg:grid-cols-2 gap-16 items-center">
            <div class="space-y-8">
              <div class="space-y-6">
                <div
                  class="inline-flex items-center bg-blue-900/50 text-blue-300 px-4 py-2 rounded-full text-sm font-medium border border-blue-400/30"
                >
                  <span class="w-2 h-2 bg-blue-400 rounded-full mr-2"></span>
                  Personal Productivity Platform
                </div>

                <h1
                  class="text-5xl lg:text-6xl font-bold text-white leading-tight"
                >
                  Master Your
                  <span class="text-blue-400 block">Productivity</span>
                </h1>

                <p class="text-xl text-blue-100 leading-relaxed max-w-lg">
                  Streamline your tasks, track expenses, and monitor your mood
                  in one powerful, intuitive platform designed for peak
                  performance.
                </p>
              </div>

              <div class="flex flex-col sm:flex-row gap-4">
                <button
                  @click="$emit('open-auth-modal')"
                  class="bg-blue-600 text-white px-8 py-4 rounded-lg text-lg font-semibold hover:bg-blue-700 transform hover:scale-105 transition-all duration-200 shadow-lg"
                >
                  Start Free Trial
                </button>
                <button
                  @click="$emit('enter-app')"
                  class="border-2 border-blue-600 text-blue-600 px-8 py-4 rounded-lg text-lg font-semibold hover:bg-blue-50 transition-colors"
                >
                  View Demo
                </button>
              </div>

              <div class="flex items-center space-x-8 pt-8">
                <div class="text-center">
                  <div class="text-2xl font-bold text-white">10K+</div>
                  <div class="text-sm text-blue-200">Active Users</div>
                </div>
                <div class="text-center">
                  <div class="text-2xl font-bold text-white">99.9%</div>
                  <div class="text-sm text-blue-200">Uptime</div>
                </div>
                <div class="text-center">
                  <div class="text-2xl font-bold text-white">4.9★</div>
                  <div class="text-sm text-blue-200">Rating</div>
                </div>
              </div>
            </div>

            <div class="relative">
              <div
                class="bg-gradient-to-r from-blue-600 to-blue-500 rounded-2xl p-8 shadow-2xl border border-blue-400/30"
              >
                <div
                  class="bg-slate-900/80 rounded-lg p-6 space-y-4 border border-blue-400/20"
                >
                  <div class="flex items-center justify-between mb-6">
                    <h3 class="text-lg font-semibold text-white">
                      Today's Overview
                    </h3>
                    <span class="text-blue-300 text-sm font-medium"
                      >Live Data</span
                    >
                  </div>

                  <div class="space-y-4">
                    <div
                      class="flex items-center justify-between p-4 bg-blue-900/50 rounded-lg border border-blue-400/30"
                    >
                      <div class="flex items-center space-x-3">
                        <div
                          class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center"
                        >
                          <svg
                            class="w-4 h-4 text-white"
                            fill="currentColor"
                            viewBox="0 0 20 20"
                          >
                            <path
                              fill-rule="evenodd"
                              d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                              clip-rule="evenodd"
                            ></path>
                          </svg>
                        </div>
                        <span class="text-white font-medium"
                          >Tasks Completed</span
                        >
                      </div>
                      <span class="text-blue-300 font-semibold">8/12</span>
                    </div>

                    <div
                      class="flex items-center justify-between p-4 bg-green-900/50 rounded-lg border border-green-400/30"
                    >
                      <div class="flex items-center space-x-3">
                        <div
                          class="w-8 h-8 bg-green-600 rounded-lg flex items-center justify-center"
                        >
                          <svg
                            class="w-4 h-4 text-white"
                            fill="currentColor"
                            viewBox="0 0 20 20"
                          >
                            <path
                              d="M4 4a2 2 0 00-2 2v4a2 2 0 002 2V6h10a2 2 0 00-2-2H4zm2 6a2 2 0 012-2h8a2 2 0 012 2v4a2 2 0 01-2 2H8a2 2 0 01-2-2v-4zm6 4a2 2 0 100-4 2 2 0 000 4z"
                            ></path>
                          </svg>
                        </div>
                        <span class="text-white font-medium"
                          >Budget Tracking</span
                        >
                      </div>
                      <span class="text-green-300 font-semibold">$2,847</span>
                    </div>

                    <div
                      class="flex items-center justify-between p-4 bg-yellow-900/50 rounded-lg border border-yellow-400/30"
                    >
                      <div class="flex items-center space-x-3">
                        <div
                          class="w-8 h-8 bg-yellow-600 rounded-lg flex items-center justify-center"
                        >
                          <svg
                            class="w-4 h-4 text-white"
                            fill="currentColor"
                            viewBox="0 0 20 20"
                          >
                            <path
                              fill-rule="evenodd"
                              d="M10 18a8 8 0 100-16 8 8 0 000 16zM7 9a1 1 0 100-2 1 1 0 000 2zm7-1a1 1 0 11-2 0 1 1 0 012 0zm-.464 5.535a1 1 0 10-1.415-1.414 3 3 0 01-4.242 0 1 1 0 00-1.415 1.414 5 5 0 006.072 0z"
                              clip-rule="evenodd"
                            ></path>
                          </svg>
                        </div>
                        <span class="text-white font-medium">Mood Score</span>
                      </div>
                      <span class="text-yellow-300 font-semibold"
                        >Excellent</span
                      >
                    </div>
                  </div>
                </div>
              </div>

              <div
                class="absolute -bottom-4 -right-4 w-24 h-24 bg-blue-300 rounded-full opacity-20"
              ></div>
              <div
                class="absolute -top-4 -left-4 w-16 h-16 bg-blue-400 rounded-full opacity-30"
              ></div>
            </div>
          </div>
        </section>

        <section id="features" class="max-w-7xl mx-auto px-6 py-20">
          <div class="text-center mb-16">
            <h2 class="text-4xl font-bold text-white mb-4">
              Everything You Need
            </h2>
            <p class="text-xl text-blue-100 max-w-2xl mx-auto">
              Powerful features designed to boost your productivity and keep you
              organized
            </p>
          </div>

          <div class="grid md:grid-cols-3 gap-8">
            <div
              class="bg-slate-900/60 rounded-xl p-8 border border-blue-400/30 hover:border-blue-300 transition-colors group backdrop-blur-sm"
            >
              <div
                class="w-12 h-12 bg-blue-600 rounded-lg flex items-center justify-center mb-6 group-hover:scale-110 transition-transform"
              >
                <svg
                  class="w-6 h-6 text-white"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                >
                  <path
                    fill-rule="evenodd"
                    d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                    clip-rule="evenodd"
                  ></path>
                </svg>
              </div>
              <h3 class="text-xl font-semibold text-white mb-3">
                Smart Task Management
              </h3>
              <p class="text-blue-100">
                Organize, prioritize, and track your tasks with intelligent
                scheduling and deadline management.
              </p>
            </div>

            <div
              class="bg-slate-900/60 rounded-xl p-8 border border-blue-400/30 hover:border-blue-300 transition-colors group backdrop-blur-sm"
            >
              <div
                class="w-12 h-12 bg-blue-600 rounded-lg flex items-center justify-center mb-6 group-hover:scale-110 transition-transform"
              >
                <svg
                  class="w-6 h-6 text-white"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                >
                  <path
                    d="M4 4a2 2 0 00-2 2v4a2 2 0 002 2V6h10a2 2 0 00-2-2H4zm2 6a2 2 0 012-2h8a2 2 0 012 2v4a2 2 0 01-2 2H8a2 2 0 01-2-2v-4zm6 4a2 2 0 100-4 2 2 0 000 4z"
                  ></path>
                </svg>
              </div>
              <h3 class="text-xl font-semibold text-white mb-3">
                Expense Tracking
              </h3>
              <p class="text-blue-100">
                Monitor your spending patterns, set budgets, and get insights
                into your financial habits.
              </p>
            </div>

            <div
              class="bg-slate-900/60 rounded-xl p-8 border border-blue-400/30 hover:border-blue-300 transition-colors group backdrop-blur-sm"
            >
              <div
                class="w-12 h-12 bg-blue-600 rounded-lg flex items-center justify-center mb-6 group-hover:scale-110 transition-transform"
              >
                <svg
                  class="w-6 h-6 text-white"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                >
                  <path
                    fill-rule="evenodd"
                    d="M10 18a8 8 0 100-16 8 8 0 000 16zM7 9a1 1 0 100-2 1 1 0 000 2zm7-1a1 1 0 11-2 0 1 1 0 012 0zm-.464 5.535a1 1 0 10-1.415-1.414 3 3 0 01-4.242 0 1 1 0 00-1.415 1.414 5 5 0 006.072 0z"
                    clip-rule="evenodd"
                  ></path>
                </svg>
              </div>
              <h3 class="text-xl font-semibold text-white mb-3">
                Mood Analytics
              </h3>
              <p class="text-blue-100">
                Track your emotional well-being and discover patterns that
                affect your productivity.
              </p>
            </div>
          </div>
        </section>

        <section id="about" class="bg-slate-900/30 py-20 backdrop-blur-sm">
          <div class="max-w-7xl mx-auto px-6">
            <div class="grid lg:grid-cols-2 gap-16 items-center">
              <div>
                <h2 class="text-4xl font-bold text-white mb-6">
                  Built for Peak Performance
                </h2>
                <p class="text-lg text-blue-100 mb-8">
                  FlowBit combines the power of task management, financial
                  tracking, and mood monitoring into one seamless experience.
                  Our platform is designed to help you achieve your goals while
                  maintaining a healthy work-life balance.
                </p>
                <div class="grid grid-cols-2 gap-6">
                  <div>
                    <div class="text-3xl font-bold text-blue-400 mb-2">
                      99.9%
                    </div>
                    <div class="text-blue-100">Uptime Guarantee</div>
                  </div>
                  <div>
                    <div class="text-3xl font-bold text-blue-400 mb-2">
                      24/7
                    </div>
                    <div class="text-blue-100">Support Available</div>
                  </div>
                </div>
              </div>
              <div class="relative">
                <div
                  class="bg-slate-900/80 rounded-2xl p-8 shadow-xl border border-blue-400/30 backdrop-blur-sm"
                >
                  <div class="space-y-6">
                    <div class="flex items-center space-x-4">
                      <div class="w-3 h-3 bg-green-400 rounded-full"></div>
                      <span class="text-blue-100"
                        >Real-time synchronization</span
                      >
                    </div>
                    <div class="flex items-center space-x-4">
                      <div class="w-3 h-3 bg-blue-400 rounded-full"></div>
                      <span class="text-blue-100">Advanced analytics</span>
                    </div>
                    <div class="flex items-center space-x-4">
                      <div class="w-3 h-3 bg-purple-400 rounded-full"></div>
                      <span class="text-blue-100"
                        >Cross-platform compatibility</span
                      >
                    </div>
                    <div class="flex items-center space-x-4">
                      <div class="w-3 h-3 bg-yellow-400 rounded-full"></div>
                      <span class="text-blue-100">Bank-grade security</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>
      </main>

      <footer
        class="bg-slate-900/50 border-t border-blue-400/20 py-12 backdrop-blur-sm"
      >
        <div class="max-w-7xl mx-auto px-6">
          <div class="grid md:grid-cols-4 gap-8">
            <div class="col-span-2">
              <div class="flex items-center space-x-3 mb-4">
                <div
                  class="w-8 h-8 bg-gradient-to-r from-blue-600 to-blue-500 rounded-lg flex items-center justify-center"
                >
                  <div class="w-4 h-4 bg-white rounded-sm"></div>
                </div>
                <span class="text-2xl font-bold text-white">FlowBit</span>
              </div>
              <p class="text-blue-100 max-w-md">
                The ultimate productivity platform for modern professionals.
                Streamline your workflow and achieve more.
              </p>
            </div>

            <div>
              <h4 class="font-semibold text-white mb-4">Product</h4>
              <div class="space-y-2">
                <a
                  href="#"
                  class="block text-blue-200 hover:text-white transition-colors"
                  >Features</a
                >
                <a
                  href="#"
                  class="block text-blue-200 hover:text-white transition-colors"
                  >Pricing</a
                >
                <a
                  href="#"
                  class="block text-blue-200 hover:text-white transition-colors"
                  >Documentation</a
                >
              </div>
            </div>

            <div>
              <h4 class="font-semibold text-white mb-4">Company</h4>
              <div class="space-y-2">
                <a
                  href="#"
                  class="block text-blue-200 hover:text-white transition-colors"
                  >About</a
                >
                <a
                  href="#"
                  class="block text-blue-200 hover:text-white transition-colors"
                  >Contact</a
                >
                <a
                  href="#"
                  class="block text-blue-200 hover:text-white transition-colors"
                  >Privacy</a
                >
              </div>
            </div>
          </div>

          <div
            class="border-t border-blue-400/20 mt-12 pt-8 flex flex-col md:flex-row justify-between items-center"
          >
            <p class="text-blue-200">© 2025 FlowBit. All rights reserved.</p>
            <div class="flex space-x-6 mt-4 md:mt-0">
              <a
                href="#"
                class="text-blue-300 hover:text-white transition-colors"
              >
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path
                    d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"
                  />
                </svg>
              </a>
              <a
                href="#"
                class="text-blue-300 hover:text-white transition-colors"
              >
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path
                    d="M22.46 6c-.77.35-1.6.58-2.46.69.88-.53 1.56-1.37 1.88-2.38-.83.5-1.75.85-2.72 1.05C18.37 4.5 17.26 4 16 4c-2.35 0-4.27 1.92-4.27 4.29 0 .34.04.67.11.98C8.28 9.09 5.11 7.38 3 4.79c-.37.63-.58 1.37-.58 2.15 0 1.49.75 2.81 1.91 3.56-.71 0-1.37-.2-1.95-.5v.03c0 2.08 1.48 3.82 3.44 4.21a4.22 4.22 0 0 1-1.93.07 4.28 4.28 0 0 0 4 2.98 8.521 8.521 0 0 1-5.33 1.84c-.34 0-.68-.02-1.02-.06C3.44 20.29 5.7 21 8.12 21 16 21 20.33 14.46 20.33 8.79c0-.19 0-.37-.01-.56.84-.6 1.56-1.36 2.14-2.23z"
                  />
                </svg>
              </a>
            </div>
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>

<script setup>
defineEmits(['open-auth-modal', 'enter-app', 'go-to-landing'])
</script>
