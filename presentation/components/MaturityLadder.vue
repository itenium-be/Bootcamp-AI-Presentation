<template>
  <div class="ladder">
    <div
      v-for="(item, i) in items"
      :key="i"
      class="rung"
      :class="{ dimmed: isHighlightActive && !isInHighlight(i) }"
      :style="rungStyle(i)"
      v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
    >
      <span class="rung-num">{{ prefix }}{{ startAt + i }}</span>
      <span class="rung-label">{{ item }}</span>
    </div>
    <div
      v-if="highlight"
      class="highlight"
      :style="highlightStyle"
      v-click="highlightClickIdx"
    />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'

const { clicks } = useNav()

const props = withDefaults(defineProps<{
  items: string[]
  startAt?: number
  prefix?: string
  firstVisible?: number
  rungWidth?: number   // percent of container
  palette?: string[]
  highlight?: [number, number]  // [fromIndex, toIndex] inclusive, zero-based
  highlightColor?: string
  highlightPadding?: number     // percent
}>(), {
  startAt: 0,
  prefix: 'L',
  firstVisible: 0,
  rungWidth: 55,
  highlight: undefined,
  highlightColor: '#E78200',
  highlightPadding: 1.5,
  palette: () => [
    '#6fbda5', // theme green
    '#7eb39a',
    '#9aa982',
    '#c19c5e',
    '#d6883d',
    '#E78200', // theme orange
    '#cc5a1e',
    '#a83419',
    '#7a1a10',
  ],
})

const total = computed(() => props.items.length)
const stepX = computed(() => (100 - props.rungWidth) / Math.max(total.value - 1, 1))
const stepY = computed(() => 100 / total.value)

function rungStyle(i: number) {
  return {
    bottom: `${i * stepY.value}%`,
    left: `${i * stepX.value}%`,
    width: `${props.rungWidth}%`,
    height: `${stepY.value * 0.85}%`,
    background: props.palette[i % props.palette.length],
    zIndex: i,
  }
}

const highlightStyle = computed(() => {
  if (!props.highlight) return {}
  const [from, to] = props.highlight
  const pad = props.highlightPadding
  const left = from * stepX.value - pad
  const bottom = from * stepY.value - pad
  const width = (to - from) * stepX.value + props.rungWidth + 2 * pad
  const height = (to - from + 0.85) * stepY.value + 2 * pad
  return {
    left: `${left}%`,
    bottom: `${bottom}%`,
    width: `${width}%`,
    height: `${height}%`,
    '--ants-color': props.highlightColor,
  }
})

const highlightClickIdx = computed(() => props.items.length - props.firstVisible + 1)

const isHighlightActive = computed(
  () => !!props.highlight && clicks.value >= highlightClickIdx.value,
)

function isInHighlight(i: number) {
  if (!props.highlight) return false
  const [from, to] = props.highlight
  return i >= from && i <= to
}
</script>

<style scoped>
.ladder {
  position: relative;
  width: 100%;
  height: 100%;
  min-height: 22rem;
}

.rung {
  position: absolute;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0 1rem;
  color: white;
  font-family: 'Inter', sans-serif;
  font-size: 1rem;
  font-weight: 500;
  border-radius: 4px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  transition: opacity 0.35s ease, filter 0.35s ease;
}

.rung.dimmed {
  opacity: 0.25;
  filter: grayscale(0.7);
}

.rung-num {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.85em;
  background: rgba(0, 0, 0, 0.18);
  padding: 0.1em 0.5em;
  border-radius: 3px;
  flex-shrink: 0;
}

.rung-label {
  font-size: 0.95em;
  overflow: hidden;
  text-overflow: ellipsis;
}

.highlight {
  position: absolute;
  --ants-color: #E78200;
  --ants-size: 18px;
  --ants-thickness: 5px;
  background-image:
    linear-gradient(90deg, var(--ants-color) 50%, transparent 50%),
    linear-gradient(90deg, var(--ants-color) 50%, transparent 50%),
    linear-gradient(0deg,  var(--ants-color) 50%, transparent 50%),
    linear-gradient(0deg,  var(--ants-color) 50%, transparent 50%);
  background-repeat: repeat-x, repeat-x, repeat-y, repeat-y;
  background-size:
    var(--ants-size) var(--ants-thickness),
    var(--ants-size) var(--ants-thickness),
    var(--ants-thickness) var(--ants-size),
    var(--ants-thickness) var(--ants-size);
  background-position: 0 0, 0 100%, 0 0, 100% 0;
  border-radius: 8px;
  pointer-events: none;
  z-index: 100;
  box-sizing: border-box;
  animation: marching-ants 0.7s linear infinite;
  filter: drop-shadow(0 0 12px rgba(231, 130, 0, 0.45));
}

@keyframes marching-ants {
  to {
    background-position:
      var(--ants-size) 0,
      calc(-1 * var(--ants-size)) 100%,
      0 calc(-1 * var(--ants-size)),
      100% var(--ants-size);
  }
}
</style>
