<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import { RocketIcon } from 'lucide-vue-next'
  
  const canvas = ref(null)
  const cursor = ref(null)
  let animationFrameId = null
  let ctx = null
  
  class Neuron {
    constructor(x, y) {
      this.x = x
      this.y = y
      this.connections = []
      this.size = Math.random() * 3 + 1
      this.speed = Math.random() * 0.5 + 0.1
      this.angle = Math.random() * Math.PI * 2
    }
  
    update(width, height) {
      this.x += Math.cos(this.angle) * this.speed
      this.y += Math.sin(this.angle) * this.speed
  
      if (this.x < 0 || this.x > width) this.angle = Math.PI - this.angle
      if (this.y < 0 || this.y > height) this.angle = -this.angle
    }
  }
  
const initNeurons = (width, height) => {
    const neurons = []
    const numNeurons = 100
    const centerX = width / 2
    const centerY = height / 2
  
    for (let i = 0; i < numNeurons; i++) {
      const angle = Math.random() * Math.PI * 2
      const distance = Math.random() * (width / 2)
      neurons.push(new Neuron(
        centerX + Math.cos(angle) * distance,
        centerY + Math.sin(angle) * distance
      ))
    }
  
    return neurons
  }
  
const animate = (neurons) => {
    if (!ctx || !canvas.value) return
  
    ctx.clearRect(0, 0, canvas.value.width, canvas.value.height)
    
    const gradient = ctx.createRadialGradient(
      canvas.value.width / 2, canvas.value.height / 2, 0,
      canvas.value.width / 2, canvas.value.height / 2, canvas.value.width / 2
    )
    gradient.addColorStop(0, 'rgba(255, 255, 255, 0.4)')
    gradient.addColorStop(1, 'rgba(255, 255, 255, 0)')
    
    ctx.fillStyle = gradient
    ctx.fillRect(0, 0, canvas.value.width, canvas.value.height)
  
    ctx.strokeStyle = 'rgba(255, 255, 255, 0.7)'
    ctx.lineWidth = 0.5
  
    neurons.forEach(neuron => {
      neuron.update(canvas.value.width, canvas.value.height)
  
      ctx.beginPath()
      ctx.arc(neuron.x, neuron.y, neuron.size, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(255, 255, 255, 0.9)'
      ctx.fill()
  
      neurons.forEach(otherNeuron => {
        if (neuron !== otherNeuron) {
          const dx = otherNeuron.x - neuron.x
          const dy = otherNeuron.y - neuron.y
          const distance = Math.sqrt(dx * dx + dy * dy)
  
          if (distance < 100) {
            ctx.beginPath()
            ctx.moveTo(neuron.x, neuron.y)
            ctx.lineTo(otherNeuron.x, otherNeuron.y)
            ctx.globalAlpha = 1 - distance / 100
            ctx.stroke()
          }
        }
      })
    })
  
    animationFrameId = requestAnimationFrame(() => animate(neurons))
  }
  
onMounted(() => {
    if (canvas.value) {
      ctx = canvas.value.getContext('2d')
      canvas.value.width = window.innerWidth
      canvas.value.height = window.innerHeight
  
      const neurons = initNeurons(canvas.value.width, canvas.value.height)
      animate(neurons)
  
      const handleResize = () => {
        canvas.value.width = window.innerWidth
        canvas.value.height = window.innerHeight
        neurons.length = 0
        neurons.push(...initNeurons(canvas.value.width, canvas.value.height))
      }
      window.addEventListener('resize', handleResize)
  
      // Cursor effect
      const handleMouseMove = (e) => {
        if (cursor.value) {
          cursor.value.style.transform = `translate(${e.clientX}px, ${e.clientY}px)`;
        }
      }
      const handleMouseEnter = () => {
        if (cursor.value) {
          cursor.value.classList.add('hovering');
        }
      }
  
      const handleMouseLeave = () => {
        if (cursor.value) {
          cursor.value.classList.remove('hovering');
        }
      }
  
      window.addEventListener('mousemove', handleMouseMove)
      document.querySelectorAll('button, a').forEach(el => {
        el.addEventListener('mouseenter', handleMouseEnter);
        el.addEventListener('mouseleave', handleMouseLeave);
      });
    }
  })
  
onUnmounted(() => {
    if (animationFrameId) {
      cancelAnimationFrame(animationFrameId)
    }
    window.removeEventListener('resize', handleResize)
    window.removeEventListener('mousemove', handleMouseMove)
    document.querySelectorAll('button, a').forEach(el => {
      el.removeEventListener('mouseenter', handleMouseEnter);
      el.removeEventListener('mouseleave', handleMouseLeave);
    });
  })
const handleDeploy = () => {
  console.log('Deploying...')
}
</script>
<template>
    <div class="min-h-screen bg-black text-white relative overflow-hidden">
      <!-- Animated neural network background -->
      <canvas ref="canvas" class="absolute inset-0 w-full h-full opacity-40"></canvas>
  
      <!-- Cursor follower -->
      <div ref="cursor" class="cursor-follower"></div>
  
      <!-- Content -->
      <div class="relative z-10">
        <!-- Navigation -->
        <nav class="px-6 py-4 flex items-center justify-between">    
          <div class="flex justify-center item-center gap-1 ">
            <div><img src="/l1.svg" class="size-12"></div>
            <span class="text-2xl font-bold mt-1">mlops.ai</span>
            </div>
          <div class="flex items-center gap-4">
            <button class="px-4 py-2 bg-transparent border border-white text-white rounded-md hover:bg-white hover:text-black transition-colors">
              Login
            </button>
            <button class="px-4 py-2 bg-white text-black rounded-md hover:bg-gray-200 transition-colors">
              Sign Up
            </button>
          </div>
        </nav>
  
        <!-- Hero Section -->
        <main class="flex flex-col items-center justify-center min-h-[80vh] px-4 text-center">
          <div class="bg-white/5 backdrop-blur-sm px-6 py-2 rounded-full mb-6">
            Optimized Testing for Complex LLMs
          </div>
          <div class=" text-5xl md:text-7xl font-bold mb-8 max-w-5xl">
            Your Models - Our Platform<br>
            <div class="mt-3">
                Endless Possibilities
            </div>
          </div>
          <button 
            @click="handleDeploy"
            class="group relative inline-flex items-center justify-center bg-blue-500 text-white px-6 py-3 rounded-lg text-lg font-medium hover:bg-blue-600 transition-all duration-200 animate-fade-in-up animation-delay-600"
          >
            <RocketIcon class="h-5 w-5 mr-2 transition-transform group-hover:-translate-y-1" />
            Get Started
          </button>
  
          <!-- AI/LLM Icon container -->
          <div class="mt-8 relative">
          <div class="w-32 h-32 bg-black border border-gray-800 rounded-2xl flex items-center justify-center relative glow-container">
            <img src="/l1.svg" class="size-24">    
          </div>
        </div>
        </main>
      </div>
    </div>
  </template>
  

  