<template>
  <div class="rs-cmp">
    <!-- DON'T side -->
    <div class="rs-side rs-side-dont">
      <h3 class="rs-header rs-header-dont" v-click="cl(1)">
        <span class="rs-mark">✕</span> Don't
      </h3>
      <div class="rs-stage rs-stage-vert">
        <div class="rs-box rs-box-mono" v-click="cl(1)">
          <div class="rs-box-title">Model A</div>
          <div class="rs-pill">implement</div>
          <div class="rs-plus">+</div>
          <div class="rs-pill">review</div>
        </div>

        <svg class="rs-arrow rs-arrow-v" viewBox="0 0 30 100" preserveAspectRatio="xMidYMid meet" v-click="cl(2)">
          <line x1="15" y1="4" x2="15" y2="80" stroke="#c97464" stroke-width="3" stroke-linecap="round" />
          <polygon points="15,96 5,78 25,78" fill="#c97464" />
        </svg>

        <div class="rs-bubble rs-bubble-dont rs-bubble-down" v-click="cl(2)">LGTM!</div>
      </div>
    </div>

    <div class="rs-divider"></div>

    <!-- DO side -->
    <div class="rs-side rs-side-do">
      <h3 class="rs-header rs-header-do" v-click="cl(3)">
        <span class="rs-mark">✓</span> Do
      </h3>
      <div class="rs-stage rs-stage-vert">
        <div class="rs-box rs-box-impl" v-click="cl(3)">
          Model A — <strong>implement</strong>
        </div>

        <div class="rs-pr-wrap" v-click="cl(4)">
          <svg class="rs-arrow rs-arrow-v" viewBox="0 0 30 100" preserveAspectRatio="xMidYMid meet">
            <line x1="15" y1="4" x2="15" y2="80" stroke="#c97464" stroke-width="3" stroke-linecap="round" />
            <polygon points="15,96 5,78 25,78" fill="#c97464" />
          </svg>
          <span class="rs-pr-label">PR</span>
        </div>

        <div class="rs-box rs-box-review" v-click="cl(4)">
          Model B — <strong>review</strong>
        </div>

        <svg class="rs-arrow rs-arrow-v" viewBox="0 0 30 100" preserveAspectRatio="xMidYMid meet" v-click="cl(5)">
          <line x1="15" y1="4" x2="15" y2="80" stroke="#6f9d83" stroke-width="3" stroke-linecap="round" />
          <polygon points="15,96 5,78 25,78" fill="#6f9d83" />
        </svg>

        <div class="rs-bubble rs-bubble-do rs-bubble-down" v-click="cl(5)">3 issues found</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = withDefaults(defineProps<{
  firstVisible?: number
}>(), {
  firstVisible: 0,
})

function cl(n: number) {
  const v = n - props.firstVisible
  return v <= 0 ? undefined : v
}
</script>

<style scoped>
.rs-cmp {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  gap: 2rem;
  width: 92%;
  margin: 1.5rem auto;
  align-items: stretch;
  font-family: 'Inter', sans-serif;
}

.rs-side {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  padding: 1rem 0;
}

.rs-divider {
  width: 1px;
  background: rgba(0, 0, 0, 0.18);
  align-self: stretch;
}

.rs-header {
  font-family: 'Rubik', sans-serif;
  font-size: 1.7rem;
  font-weight: 700;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 0.45rem;
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.rs-header.slidev-vclick-hidden {
  opacity: 0;
  transform: translateY(-10px);
}

.rs-header-dont { color: #c97464; }
.rs-header-do   { color: #6f9d83; }

.rs-mark {
  font-size: 1.3em;
  font-weight: 800;
}

.rs-stage {
  display: flex;
  align-items: center;
  gap: 0.9rem;
  flex: 1;
}

.rs-stage-vert {
  flex-direction: column;
  align-items: stretch;
  gap: 0.4rem;
  width: 100%;
}

.rs-box {
  color: white;
  padding: 0.85rem 1.1rem;
  border-radius: 8px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.18);
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.rs-box.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.7);
}

.rs-box-mono {
  background: #5a6578;
  align-self: center;
  min-width: 70%;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.35rem;
  padding: 1rem 1.2rem;
}

.rs-box-title {
  font-weight: 700;
  font-size: 1.05rem;
  margin-bottom: 0.4rem;
}

.rs-pill {
  background: rgba(255, 255, 255, 0.18);
  padding: 0.25rem 0.85rem;
  border-radius: 4px;
  font-size: 0.9rem;
  min-width: 5.5rem;
  text-align: center;
}

.rs-plus {
  font-size: 0.85rem;
  opacity: 0.7;
  line-height: 1;
}

.rs-box-impl {
  background: #c97464;
  font-size: 1.1rem;
  text-align: center;
  align-self: center;
  min-width: 70%;
}

.rs-box-review {
  background: #6f9d83;
  font-size: 1.1rem;
  text-align: center;
  align-self: center;
  min-width: 70%;
}

.rs-arrow {
  flex-shrink: 0;
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.rs-arrow.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.6);
}

.rs-arrow-h { width: 60px; height: 24px; }
.rs-arrow-v { width: 24px; height: 56px; align-self: center; }

.rs-pr-wrap {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  align-self: center;
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.rs-pr-label {
  position: absolute;
  left: calc(50% + 22px);
  top: 50%;
  transform: translateY(-50%);
}

.rs-pr-wrap.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.7);
}

.rs-pr-label {
  font-weight: 700;
  font-size: 1rem;
  color: #c97464;
  font-family: 'Rubik', sans-serif;
}

.rs-review-row {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.rs-bubble {
  position: relative;
  padding: 0.55rem 1.1rem;
  border-radius: 18px;
  font-weight: 600;
  font-size: 0.95rem;
  white-space: nowrap;
  transition: opacity 0.4s ease, transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.rs-bubble.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.5) rotate(-10deg);
}

.rs-bubble::before {
  content: '';
  position: absolute;
  left: -7px;
  top: 50%;
  transform: translateY(-50%);
  border-top: 7px solid transparent;
  border-bottom: 7px solid transparent;
  border-right: 9px solid;
}

.rs-bubble-dont {
  background: #e8a097;
  color: #6e2e25;
}
.rs-bubble-dont::before { border-right-color: #e8a097; }

.rs-bubble-do {
  background: #a5cdb8;
  color: #2f4a3a;
}
.rs-bubble-do::before { border-right-color: #a5cdb8; }

.rs-bubble-down {
  align-self: center;
}

.rs-bubble-down::before {
  left: 50%;
  top: -7px;
  transform: translateX(-50%);
  border: 7px solid transparent;
  border-top: 0;
  border-right-color: transparent;
}

.rs-bubble-dont.rs-bubble-down::before { border-bottom: 9px solid #e8a097; }
.rs-bubble-do.rs-bubble-down::before   { border-bottom: 9px solid #a5cdb8; }
</style>
