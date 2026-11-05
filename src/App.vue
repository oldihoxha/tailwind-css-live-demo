<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import FeatureCard from './components/FeatureCard.vue'

// Demo-Zustände: 1 = Ausgangszustand, 2 = gestylt, 3 = interaktiv (Tasten 1/2/3)
const stages = [
  { id: 1, label: '1 · Basis' },
  { id: 2, label: '2 · Styling' },
  { id: 3, label: '3 · Interaktiv' },
]
const stage = ref(1)

const onKey = (e) => {
  const n = Number(e.key)
  if (n >= 1 && n <= stages.length) stage.value = n
}
onMounted(() => window.addEventListener('keydown', onKey))
onUnmounted(() => window.removeEventListener('keydown', onKey))
</script>

<template>
  <div
    class="min-h-screen px-4 py-10 md:px-8 md:py-16"
    :class="stage === 1 ? 'bg-white text-black' : 'bg-slate-950 text-slate-100'"
  >
    <main class="mx-auto max-w-5xl">
      <FeatureCard :stage="stage" />
    </main>

    <nav
      aria-label="Demo-Zustand"
      class="fixed inset-x-0 bottom-4 mx-auto flex w-fit gap-1 rounded-full border border-slate-700 bg-slate-900/90 p-1 text-sm shadow-lg backdrop-blur"
    >
      <button
        v-for="s in stages"
        :key="s.id"
        type="button"
        :aria-pressed="stage === s.id"
        class="rounded-full px-4 py-2 font-medium text-slate-300 transition-colors hover:text-white focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-teal-300 aria-pressed:bg-teal-400 aria-pressed:text-slate-950"
        @click="stage = s.id"
      >
        {{ s.label }}
      </button>
    </nav>
  </div>
</template>
