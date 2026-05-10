<template>
  <span class="fire-text"><slot /></span>
  <svg aria-hidden="true" class="fire-text-defs">
    <filter id="fire-filter">
      <feTurbulence type="fractalNoise" baseFrequency="0.02 0.06" numOctaves="2" seed="2" result="noise">
        <animate attributeName="baseFrequency" dur="6s" values="0.02 0.06;0.05 0.12;0.02 0.06" repeatCount="indefinite" />
        <animate attributeName="seed" dur="3s" values="1;9;3;7;1" repeatCount="indefinite" />
      </feTurbulence>
      <feDisplacementMap in="SourceGraphic" in2="noise" scale="6" />
    </filter>
  </svg>
</template>

<style>
.fire-text-defs {
  position: absolute;
  width: 0;
  height: 0;
}
.fire-text {
  background: linear-gradient(180deg, #fde047 0%, #fb923c 50%, #ef4444 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  filter: url(#fire-filter) drop-shadow(0 0 12px #fb923c);
  animation: fire-glow 2.4s ease-in-out infinite alternate;
}
@keyframes fire-glow {
  from { filter: url(#fire-filter) drop-shadow(0 0 6px #fb923c); }
  to   { filter: url(#fire-filter) drop-shadow(0 0 18px #ef4444); }
}
</style>
