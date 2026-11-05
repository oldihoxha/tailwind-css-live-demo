<script setup>
import { ref, computed } from 'vue'

const props = defineProps({ stage: { type: Number, default: 3 } })

const active = ref(false)
const interactive = computed(() => props.stage === 3)

// Utility-Klassen je Demo-Zustand (vollständige Strings, damit Tailwind sie findet)
const styles = {
  1: {
    card: 'border border-gray-400 p-4',
    badge: 'border border-gray-400 px-1',
    title: 'text-xl font-bold',
    text: '',
    button: 'border border-gray-500 bg-gray-200 px-2 py-1',
  },
  2: {
    card: 'grid gap-6 rounded-2xl border bg-slate-900 p-6 transition-colors md:grid-cols-[auto_1fr_auto] md:items-center md:gap-8 md:p-8',
    badge: 'rounded-full px-3 py-1 text-sm font-medium',
    title: 'text-2xl font-bold text-white md:text-3xl',
    text: 'mt-2 text-slate-300',
    button:
      'rounded-xl px-6 py-3 font-semibold transition-colors focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-teal-300',
    buttonOff: 'bg-teal-400 text-slate-950 hover:bg-teal-300',
    buttonOn: 'text-teal-300 ring-1 ring-teal-400 hover:bg-teal-400/10',
  },
}
styles[3] = styles[2]

const s = computed(() => styles[props.stage])
const on = computed(() => active.value && interactive.value)
const muted = computed(() => interactive.value && !active.value)
const cardColor = computed(() => {
  if (props.stage === 1) return ''
  if (on.value) return 'border-teal-400/50 shadow-xl shadow-teal-400/25'
  return muted.value ? 'border-slate-800' : 'border-slate-700 shadow-xl shadow-teal-500/10'
})
const iconColor = computed(() =>
  muted.value ? 'bg-slate-800 text-slate-500 ring-slate-700' : 'bg-teal-400/10 text-teal-300 ring-teal-400/30',
)
const badgeColor = computed(() => {
  if (props.stage === 1) return ''
  return on.value
    ? 'bg-teal-500/15 text-teal-300'
    : 'bg-slate-700/60 text-slate-300'
})
const buttonColor = computed(() => {
  if (props.stage === 1) return ''
  return on.value ? s.value.buttonOn : s.value.buttonOff
})
const statusText = computed(() => (on.value ? 'Aktiviert' : 'Deaktiviert'))

function toggle() {
  if (interactive.value) active.value = !active.value
}
</script>

<template>
  <article :class="[s.card, cardColor]">
    <div
      v-if="stage > 1"
      :class="iconColor"
      class="flex size-14 items-center justify-center rounded-xl ring-1 transition-colors"
      aria-hidden="true"
    >
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="size-7">
        <path d="M13 2 3 14h9l-1 8 10-12h-9l1-8z" />
      </svg>
    </div>

    <div>
      <span :class="[s.badge, badgeColor]" aria-live="polite">{{ statusText }}</span>
      <h1 class="mt-4" :class="s.title">Schneller zu modernen UIs</h1>
      <p :class="s.text">
        Eine Vue-Komponente, gestaltet mit Tailwind-Utilities – ohne eine Zeile eigenes CSS.
      </p>
    </div>

    <button
      type="button"
      :class="[s.button, buttonColor, stage > 1 && !interactive ? 'cursor-not-allowed opacity-60' : '']"
      :aria-pressed="interactive ? active : undefined"
      :aria-disabled="stage > 1 && !interactive ? true : undefined"
      @click="toggle"
    >
      {{ on ? 'Deaktivieren' : 'Aktivieren' }}
    </button>
  </article>
</template>
