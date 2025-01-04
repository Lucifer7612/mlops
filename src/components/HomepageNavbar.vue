<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { ActivityIcon, UserIcon, RocketIcon } from 'lucide-vue-next'

const mousePosition = ref({ x: 0, y: 0 })

const backgroundStyle = computed(() => ({
  background: `radial-gradient(circle at ${mousePosition.value.x}px ${mousePosition.value.y}px, rgba(59, 130, 246, 0.3) 0%, rgba(147, 51, 234, 0.1) 35%, transparent 70%)`,
  transition: 'all 0.3s ease-out'
}))

const handleMouseMove = (event) => {
  mousePosition.value = { x: event.clientX, y: event.clientY }
}

const handleDeploy = () => {
  console.log('Deploying...')
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove)
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
})
</script>

<template>
  <div class="min-h-screen overflow-hidden bg-black text-white">
    <!-- Animated background element -->
    <div 
      class="fixed inset-0 pointer-events-none"
      :style="backgroundStyle"
    ></div>

    <!-- Content -->
    <div class="relative z-10 backdrop-blur-sm">
      <!-- Navbar -->
      <nav class="border-b border-white/10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div class="flex justify-between h-16 items-center">
            <!-- Logo -->
            <div class="flex items-center">
              <a href="/" class="flex items-center group">
                <ActivityIcon class="h-8 w-8 text-blue-500 group-hover:text-blue-400 transition-colors" />
                <span class="ml-2 text-2xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-500 to-purple-500 group-hover:from-blue-400 group-hover:to-purple-400 transition-all">
                  mlops.ai
                </span>
              </a>
            </div>
            <!-- Auth Buttons -->
            <div class="flex items-center space-x-4">
              <button class="text-white font-medium hover:text-blue-400 transition-colors flex items-center">
                <UserIcon class="h-5 w-5 mr-1" />
                Log in
              </button>
              <button class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 transition-colors">
                Sign Up
              </button>
            </div>
          </div>
        </div>    
      </nav>

      <!-- Main Content with Deploy Button -->
      <main class="min-h-[calc(100vh-4rem)] flex items-center justify-center px-4">
        <div class="text-center">
          <h1 class="text-5xl sm:text-6xl md:text-7xl font-bold mb-8 animate-fade-in-up">
            Welcome to 
            <span class="bg-clip-text text-transparent bg-gradient-to-r from-blue-500 to-purple-500">
              mlops.ai
            </span>
          </h1>
          <p class="text-xl sm:text-2xl text-gray-300 mb-12 max-w-2xl mx-auto animate-fade-in-up animation-delay-300">
            Revolutionize your ML workflow with cutting-edge MLOps solutions
          </p>
          <button 
            @click="handleDeploy"
            class="group relative inline-flex items-center justify-center bg-blue-500 text-white px-6 py-3 rounded-lg text-lg font-medium hover:bg-blue-600 transition-all duration-200 animate-fade-in-up animation-delay-600"
          >
            <RocketIcon class="h-5 w-5 mr-2 transition-transform group-hover:-translate-y-1" />
            Launch Your ML Pipeline
          </button>
        </div>
      </main>
    </div>
  </div>
</template>