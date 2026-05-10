<template>
  <div class="ce-cycle">
    <div
      v-for="(item, i) in items"
      :key="`box-${i}`"
      class="ce-box"
      :style="{ background: palette[i % palette.length], gridArea: BOX_AREAS[i] }"
      v-click="firstVisible > i ? undefined : i + 1 - firstVisible"
    >
      <div class="ce-title">{{ item.title }}</div>
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
  items: { title: string, sub: string }[]
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

.ce-title {
  font-family: 'Rubik', sans-serif;
  font-size: 1.8rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  margin-bottom: 0.5rem;
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
