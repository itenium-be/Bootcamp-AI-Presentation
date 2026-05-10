<template>
  <div class="ce-cycle">
    <div
      v-for="(item, i) in items"
      :key="`box-${i}`"
      class="ce-box"
      :class="{ 'ce-shine': item.shine }"
      :style="{ background: palette[i % palette.length], gridArea: BOX_AREAS[i] }"
      v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
    >
      <div class="ce-title">
        <span v-if="item.shine" class="ce-star">★</span>
        <span class="ce-title-text">{{ item.title }}</span>
      </div>
      <div class="ce-sub" v-html="item.sub" />
    </div>

    <!-- Arrow 0: top, PLAN → DELEGATE -->
    <svg class="ce-arrow" style="grid-area: 1 / 2"
         viewBox="0 0 100 30" preserveAspectRatio="xMidYMid meet"
         v-click="2 - firstVisible">
      <line x1="4" y1="15" x2="80" y2="15" :stroke="arrowColor" stroke-width="3" stroke-linecap="round" />
      <polygon points="96,15 78,5 78,25" :fill="arrowColor" />
    </svg>

    <!-- Arrow 1: right, DELEGATE → ASSESS -->
    <svg class="ce-arrow" style="grid-area: 2 / 3"
         viewBox="0 0 30 100" preserveAspectRatio="xMidYMid meet"
         v-click="3 - firstVisible">
      <line x1="15" y1="4" x2="15" y2="80" :stroke="arrowColor" stroke-width="3" stroke-linecap="round" />
      <polygon points="15,96 5,78 25,78" :fill="arrowColor" />
    </svg>

    <!-- Arrow 2: bottom, ASSESS → CODIFY -->
    <svg class="ce-arrow" style="grid-area: 3 / 2"
         viewBox="0 0 100 30" preserveAspectRatio="xMidYMid meet"
         v-click="4 - firstVisible">
      <line x1="96" y1="15" x2="20" y2="15" :stroke="arrowColor" stroke-width="3" stroke-linecap="round" />
      <polygon points="4,15 22,5 22,25" :fill="arrowColor" />
    </svg>

    <!-- Arrow 3: left, CODIFY → PLAN (closes the loop) -->
    <svg class="ce-arrow" style="grid-area: 2 / 1"
         viewBox="0 0 30 100" preserveAspectRatio="xMidYMid meet"
         v-click="5 - firstVisible">
      <line x1="15" y1="96" x2="15" y2="20" :stroke="arrowColor" stroke-width="3" stroke-linecap="round" />
      <polygon points="15,4 5,22 25,22" :fill="arrowColor" />
    </svg>
  </div>
</template>

<script setup lang="ts">
withDefaults(defineProps<{
  items: { title: string, sub: string, shine?: boolean }[]
  firstVisible?: number
  palette?: string[]
  arrowColor?: string
}>(), {
  firstVisible: 0,
  arrowColor: '#E78200',
  palette: () => ['#7c2740', '#2d3aa3', '#4a8a4f', '#3d2e7d'],
})

// Boxes occupy the corners of a 3×3 grid; middle row/column hold the arrows.
const BOX_AREAS = ['1 / 1', '1 / 3', '3 / 3', '3 / 1']
</script>

<style scoped>
.ce-cycle {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 110px minmax(0, 1fr);
  grid-template-rows: minmax(0, 1fr) 80px minmax(0, 1fr);
  gap: 1.2rem;
  width: 80%;
  aspect-ratio: 16 / 9;
  margin: 2rem auto;
}

.ce-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.2);
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.ce-box.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.7);
}

.ce-shine {
  border: 2px solid #E78200;
  animation: ce-shine-pulse 2.4s ease-in-out infinite;
}

.ce-shine.slidev-vclick-hidden {
  animation: none;
}

@keyframes ce-shine-pulse {
  0%, 100% {
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.2),
                0 0 18px rgba(231, 130, 0, 0.45);
  }
  50% {
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.2),
                0 0 38px rgba(231, 130, 0, 0.85),
                0 0 60px rgba(255, 216, 74, 0.35);
  }
}

.ce-title {
  font-family: 'Rubik', sans-serif;
  font-size: 1.8rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  margin-bottom: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.4rem;
}

.ce-shine .ce-title-text {
  background-image: linear-gradient(
    100deg,
    #ffffff 0%,
    #ffffff 35%,
    #fff3b0 50%,
    #ffd84a 55%,
    #ffffff 70%,
    #ffffff 100%
  );
  background-size: 250% 100%;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
  animation: ce-holographic 3.5s linear infinite;
}

@keyframes ce-holographic {
  0%   { background-position: 100% 0; }
  100% { background-position: -100% 0; }
}

.ce-star {
  color: #ffd84a;
  font-size: 0.9em;
  text-shadow: 0 0 8px rgba(255, 216, 74, 0.9);
  animation: ce-spin 6s linear infinite;
  display: inline-block;
}

@keyframes ce-spin {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

.ce-sub {
  font-size: 1rem;
  opacity: 0.95;
  white-space: pre-line;
}

.ce-arrow {
  width: 100%;
  height: 100%;
  transition: opacity 0.35s ease, transform 0.35s ease;
  filter: drop-shadow(0 0 6px rgba(231, 130, 0, 0.35));
}

.ce-arrow.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.7);
}
</style>
