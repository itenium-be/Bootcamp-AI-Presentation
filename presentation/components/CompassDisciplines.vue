<template>
  <div class="compass-disciplines">
    <svg class="compass-svg" viewBox="0 0 1000 600" preserveAspectRatio="xMidYMid meet">
      <defs>
        <filter id="compass-glow" filterUnits="userSpaceOnUse" x="-50" y="-50" width="1100" height="700">
          <feGaussianBlur stdDeviation="3" result="blur" />
          <feMerge>
            <feMergeNode in="blur" />
            <feMergeNode in="SourceGraphic" />
          </feMerge>
        </filter>
      </defs>

      <line
        v-for="(item, i) in items"
        :key="'spoke' + i"
        class="spoke"
        :x1="HUB_X"
        :y1="HUB_Y"
        :x2="vertex(i).x"
        :y2="vertex(i).y"
        :stroke="palette[i % palette.length]"
        stroke-width="3"
        stroke-linecap="round"
        :stroke-dasharray="radiusFor(i)"
        :stroke-dashoffset="radiusFor(i)"
        :style="{ '--spoke-len': radiusFor(i) }"
        filter="url(#compass-glow)"
        v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
      />

      <line
        v-for="(item, i) in items"
        :key="'pulse' + i"
        class="spoke-pulse"
        :x1="HUB_X"
        :y1="HUB_Y"
        :x2="vertex(i).x"
        :y2="vertex(i).y"
        stroke="#fff3b0"
        stroke-width="13"
        stroke-linecap="round"
        :style="{
          '--pulse-duration': `${3 + i * 0.45}s`,
          '--pulse-delay': `${-i * 0.7}s`,
        }"
        filter="url(#compass-glow)"
        v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
      />

    </svg>

    <div
      v-for="(item, i) in items"
      :key="'card' + i"
      class="vertex-card"
      :style="cardStyle(i)"
      v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
    >
      <span class="label-num" :style="{ background: palette[i % palette.length] }">{{ i + 1 }}</span>
      <span class="label-text">{{ item }}</span>
    </div>

    <div v-if="hubLabel" class="hub-label">
      <span class="hub-text">{{ hubLabel }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = withDefaults(defineProps<{
  items: string[]
  firstVisible?: number
  palette?: string[]
  hubLabel?: string
}>(), {
  firstVisible: 0,
  hubLabel: 'Context Engineering',
  palette: () => [
    '#6fbda5', '#7eb39a', '#9aa982', '#c19c5e',
    '#d6883d', '#E78200', '#cc5a1e',
  ],
})

const HUB_X = 500
const HUB_Y = 300
const RADIUS = 240

// Pentagon vertices: -90°, -18°, 54°, 126°, 198° (top vertex pointing up)
const ANGLES_DEG = [-90, -18, 54, 126, 198]
// Per-spoke length overrides (index-based). Index 1 = upper-right, index 4 = upper-left.
const RADIUS_OVERRIDES: Record<number, number> = {
  1: 310,
  4: 310,
}

function radiusFor(i: number) {
  return RADIUS_OVERRIDES[i] ?? RADIUS
}

function vertex(i: number) {
  const angleDeg = ANGLES_DEG[i % ANGLES_DEG.length]
  const r = radiusFor(i)
  const rad = (angleDeg * Math.PI) / 180
  return {
    x: HUB_X + r * Math.cos(rad),
    y: HUB_Y + r * Math.sin(rad),
  }
}

function cardStyle(i: number) {
  const v = vertex(i)
  const r = radiusFor(i)
  const xPct = (v.x / 1000) * 100
  const yPct = (v.y / 600) * 100
  // Direction from hub for the "fade in from hub" entrance
  const dx = (v.x - HUB_X) / r
  const dy = (v.y - HUB_Y) / r
  return {
    left: `${xPct}%`,
    top: `${yPct}%`,
    '--from-x': `${-dx * 24}px`,
    '--from-y': `${-dy * 24}px`,
  }
}
</script>

<style scoped>
.compass-disciplines {
  position: relative;
  width: 100%;
  height: 100%;
  aspect-ratio: 1000 / 600;
  max-width: 100%;
  max-height: 100%;
  margin: auto;
  transform: translateY(-70px);
}

.compass-svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  overflow: visible;
}

.spoke {
  transition: stroke-dashoffset 0.45s ease-out, opacity 0.3s ease;
}

.spoke.slidev-vclick-hidden {
  stroke-dashoffset: var(--spoke-len, 240);
  opacity: 0;
  transition: opacity 0.2s ease;
}

.spoke:not(.slidev-vclick-hidden) {
  stroke-dashoffset: 0;
}

.spoke-pulse {
  stroke-dasharray: 40 700;
  animation: pulse-travel var(--pulse-duration, 3.5s) linear infinite;
  animation-delay: var(--pulse-delay, 0s);
  transition: opacity 0.3s ease;
  pointer-events: none;
  mix-blend-mode: screen;
}

.spoke-pulse.slidev-vclick-hidden {
  opacity: 0;
}

@keyframes pulse-travel {
  from { stroke-dashoffset: 0; }
  to   { stroke-dashoffset: -740; }
}

.vertex-card {
  position: absolute;
  transform: translate(-50%, -50%);
  display: flex;
  align-items: center;
  gap: 0.4rem;
  background: white;
  padding: 0.35rem 0.7rem;
  border-radius: 4px;
  font-family: 'Inter', sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
  color: #2D2A28;
  white-space: nowrap;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.vertex-card.slidev-vclick-hidden {
  opacity: 0;
  transform: translate(-50%, -50%) translate(var(--from-x, 0), var(--from-y, 0)) scale(0.6);
}

.label-num {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.78em;
  color: white;
  padding: 0.1em 0.45em;
  border-radius: 3px;
  flex-shrink: 0;
  min-width: 1.5em;
  text-align: center;
}

.label-text {
  flex: 1;
}

.hub-label {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: #2D2A28;
  color: #E78200;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-family: 'Rubik', sans-serif;
  font-size: 0.95rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  white-space: nowrap;
  border: 2px solid #E78200;
  box-shadow: 0 0 22px rgba(231, 130, 0, 0.5);
  pointer-events: none;
  animation: hub-pulse 2.4s ease-in-out infinite;
}

@keyframes hub-pulse {
  0%, 100% { box-shadow: 0 0 16px rgba(231, 130, 0, 0.4); }
  50%      { box-shadow: 0 0 28px rgba(231, 130, 0, 0.75); }
}

.hub-text {
  background-image: linear-gradient(
    100deg,
    #E78200 0%,
    #E78200 35%,
    #fff3b0 50%,
    #ffd84a 55%,
    #E78200 70%,
    #E78200 100%
  );
  background-size: 250% 100%;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
  animation: holographic 3.5s linear infinite;
}

@keyframes holographic {
  0%   { background-position: 100% 0; }
  100% { background-position: -100% 0; }
}
</style>
