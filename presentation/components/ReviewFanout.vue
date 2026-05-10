<template>
  <div class="rf-cmp">
    <div class="rf-pr" v-click="cl(1)">PR</div>

    <svg class="rf-arrow rf-arrow-trunk" viewBox="0 0 30 80"
         preserveAspectRatio="xMidYMid meet" v-click="cl(2)">
      <line x1="15" y1="4" x2="15" y2="62" stroke="#888" stroke-width="3" stroke-linecap="round" />
      <polygon points="15,76 5,60 25,60" fill="#888" />
    </svg>

    <div class="rf-skill" v-click="cl(2)">Review Skill</div>

    <div class="rf-fanout-zone" v-click="cl(3)">
      <div class="rf-fan-stub"></div>
      <div class="rf-fan-bar-wrap">
        <div class="rf-fan-bar"></div>
      </div>

      <div class="rf-agents-row">
        <div
          v-for="(item, i) in items"
          :key="i"
          class="rf-agent-col"
        >
          <svg class="rf-arrow rf-arrow-leg" viewBox="0 0 30 80"
               preserveAspectRatio="xMidYMid meet">
            <line x1="15" y1="0" x2="15" y2="62" stroke="#888" stroke-width="3" stroke-linecap="round" />
            <polygon points="15,76 5,60 25,60" fill="#888" />
          </svg>

          <div class="rf-agent-box" :style="{ background: item.color || palette[i % palette.length] }">
            <div class="rf-agent-name">{{ item.name }}</div>
            <div class="rf-agent-role">{{ item.role }}</div>
          </div>

          <div class="rf-verdict">
            <span class="rf-check">✓</span>
            <span class="rf-slash">/</span>
            <span class="rf-cross">✕</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = withDefaults(defineProps<{
  items?: { name: string, role: string, color?: string }[]
  firstVisible?: number
  palette?: string[]
}>(), {
  firstVisible: 0,
  items: () => [
    { name: 'Hejlsberg', role: 'C# reviewer' },
    { name: 'Beck',      role: 'Test Reviewer' },
    { name: 'Mondrian',  role: 'Avalonia Reviewer' },
  ],
  palette: () => ['#8eb39e', '#5a6578', '#c97464'],
})

function cl(n: number) {
  const v = n - props.firstVisible
  return v <= 0 ? undefined : v
}
</script>

<style scoped>
.rf-cmp {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'Inter', sans-serif;
  width: 92%;
  margin: 1rem auto;
}

.rf-pr {
  background: #c97464;
  color: white;
  padding: 0.7rem 2rem;
  border-radius: 8px;
  font-weight: 700;
  font-size: 1.25rem;
  letter-spacing: 0.05em;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.18);
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.rf-skill {
  background: #5a6578;
  color: white;
  padding: 0.85rem 3rem;
  border-radius: 8px;
  font-weight: 700;
  font-size: 1.3rem;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.18);
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.rf-pr.slidev-vclick-hidden,
.rf-skill.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.7);
}

.rf-arrow {
  flex-shrink: 0;
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.rf-arrow.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.6);
}

.rf-arrow-trunk { width: 22px; height: 50px; }
.rf-arrow-leg   { width: 22px; height: 44px; }

.rf-fanout-zone {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: opacity 0.4s ease, transform 0.4s ease;
}

.rf-fanout-zone.slidev-vclick-hidden {
  opacity: 0;
  transform: translateY(-10px) scale(0.96);
}

.rf-fan-stub {
  width: 3px;
  height: 22px;
  background: #888;
}

.rf-fan-bar-wrap {
  position: relative;
  width: 100%;
  height: 3px;
}

.rf-fan-bar {
  position: absolute;
  top: 0;
  left: 16.67%;
  right: 16.67%;
  height: 3px;
  background: #888;
}

.rf-agents-row {
  display: flex;
  width: 100%;
  align-items: flex-start;
}

.rf-agent-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 0.5rem;
  box-sizing: border-box;
}

.rf-agent-box {
  color: white;
  padding: 0.85rem 1.3rem;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.18);
  min-width: 10rem;
}

.rf-agent-name {
  font-weight: 700;
  font-size: 1.1rem;
  letter-spacing: 0.02em;
}

.rf-agent-role {
  font-size: 0.85rem;
  opacity: 0.92;
  margin-top: 0.2rem;
}

.rf-verdict {
  margin-top: 0.7rem;
  font-size: 1.3rem;
  font-weight: 700;
  display: flex;
  gap: 0.4rem;
  align-items: center;
}

.rf-check { color: #4a8a4f; }
.rf-cross { color: #c0392b; }
.rf-slash { color: #2D2A28; opacity: 0.55; }
</style>
