<template>
  <div class="prompt-prism">
    <svg class="prism-svg" viewBox="0 0 1000 600" preserveAspectRatio="xMidYMid meet">
      <defs>
        <filter id="prism-glow" x="-50%" y="-50%" width="200%" height="200%">
          <feGaussianBlur stdDeviation="3" result="blur" />
          <feMerge>
            <feMergeNode in="blur" />
            <feMergeNode in="SourceGraphic" />
          </feMerge>
        </filter>
      </defs>

      <path
        v-for="(item, i) in items"
        :key="'b' + i"
        class="beam"
        :d="beamPath(i)"
        :stroke="palette[i % palette.length]"
        stroke-width="4"
        stroke-linecap="round"
        fill="none"
        filter="url(#prism-glow)"
        v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
      />
    </svg>

    <div
      v-for="(item, i) in items"
      :key="'l' + i"
      class="input-box"
      :style="boxStyle(i)"
      v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
    >
      <span class="box-num">{{ i + 1 }}</span>
      <span class="box-label">{{ item }}</span>
    </div>

    <div v-if="showResult" class="output-label">
      <span class="result-arrow">⟶</span>
      <span class="result-text">{{ resultLabel }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = withDefaults(defineProps<{
  items: string[]
  firstVisible?: number
  palette?: string[]
  showResult?: boolean
  resultLabel?: string
}>(), {
  firstVisible: 0,
  showResult: true,
  resultLabel: 'The Prompt',
  palette: () => [
    '#6fbda5', '#7eb39a', '#9aa982', '#c19c5e',
    '#d6883d', '#E78200', '#cc5a1e',
  ],
})

function boxCenterY(i: number) {
  const total = props.items.length
  const top = 60
  const bottom = 540
  const step = (bottom - top) / Math.max(total - 1, 1)
  return top + i * step
}

function boxStyle(i: number) {
  const yPct = (boxCenterY(i) / 600) * 100
  return {
    background: props.palette[i % props.palette.length],
    top: `${yPct}%`,
  }
}

function beamPath(i: number) {
  const x1 = 240
  const y1 = boxCenterY(i)
  const x2 = 830
  const y2 = 300
  // If a line would be horizontal (middle box), arc it above the output box
  if (Math.abs(y1 - y2) < 1) {
    return `M ${x1} ${y1} C 420 200, 660 200, ${x2} ${y2}`
  }
  // Otherwise: straight line
  return `M ${x1} ${y1} L ${x2} ${y2}`
}
</script>

<style scoped>
.prompt-prism {
  position: relative;
  width: 100%;
  height: 100%;
  aspect-ratio: 1000 / 600;
  max-width: 100%;
  max-height: 100%;
  margin: auto;
}

.prism-svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
}

.beam,
.input-box,
.output-label {
  transition: opacity 0.4s ease, transform 0.4s ease;
}

.beam.slidev-vclick-hidden {
  opacity: 0;
}

.input-box {
  position: absolute;
  left: 1.5%;
  width: 22%;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 0.75rem;
  color: white;
  font-family: 'Inter', sans-serif;
  font-size: 0.95rem;
  font-weight: 500;
  border-radius: 6px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.18);
  overflow: hidden;
  white-space: nowrap;
  transform: translateY(-50%);
}

.input-box::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: 6px;
  background: linear-gradient(
    to bottom,
    rgba(255, 255, 255, 0.18) 0%,
    rgba(255, 255, 255, 0) 35%,
    rgba(0, 0, 0, 0.12) 100%
  );
  pointer-events: none;
}

.input-box.slidev-vclick-hidden {
  opacity: 0;
  transform: translateY(-50%) translateX(-12px);
}

.box-num {
  position: relative;
  z-index: 1;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.8em;
  background: rgba(0, 0, 0, 0.22);
  padding: 0.12em 0.55em;
  border-radius: 3px;
  flex-shrink: 0;
  min-width: 1.6em;
  text-align: center;
}

.box-label {
  position: relative;
  z-index: 1;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
}

.output-label {
  position: absolute;
  left: 83%;
  top: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: #2D2A28;
  color: #E78200;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  border: 2px solid #E78200;
  box-shadow: 0 0 28px rgba(231, 130, 0, 0.5);
  font-family: 'Rubik', sans-serif;
  font-weight: 600;
  font-size: 1rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  white-space: nowrap;
}

.result-arrow {
  font-size: 1.3em;
  line-height: 1;
}
</style>
