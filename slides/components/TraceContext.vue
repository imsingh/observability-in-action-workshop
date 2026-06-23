<script setup lang="ts">
defineProps<{
  traceparent?: string
}>()

const defaultValue = '00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01'

const parts = [
  { label: 'version',   color: 'var(--grey-800)',      description: 'Spec version' },
  { label: 'trace-id',  color: 'var(--turquoise-500)', description: 'Unique ID for the entire request chain' },
  { label: 'parent-id', color: 'var(--maroon-600)',    description: 'ID of the current hop (span)' },
  { label: 'flags',     color: 'var(--grey-800)',      description: 'Sampling flags' },
]
</script>

<template>
  <div class="trace-context">
    <div class="tc-header">
      <span class="tc-label">traceparent</span>
    </div>
    <div class="tc-value">
      <template v-for="(part, i) in parts" :key="part.label">
        <span class="tc-separator" v-if="i > 0">-</span>
        <span class="tc-segment" :style="{ color: part.color, '--seg-color': part.color }">
          {{ (traceparent ?? defaultValue).split('-')[i] }}
        </span>
      </template>
    </div>
    <div class="tc-legend">
      <div v-for="part in parts" :key="part.label" class="tc-legend-item">
        <span class="tc-legend-dot" :style="{ background: part.color }" />
        <span class="tc-legend-name" :style="{ color: part.color }">{{ part.label }}</span>
        <span class="tc-legend-desc">{{ part.description }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.trace-context {
  display: flex;
  flex-direction: column;
  gap: var(--space-5);
}

.tc-header {
  display: flex;
  align-items: center;
  gap: var(--space-3);
}

.tc-label {
  font-family: var(--font-mono, monospace);
  font-size: 0.8em;
  font-weight: 600;
  color: var(--fg-muted);
  background: var(--bg-surface);
  padding: 0.2em 0.6em;
  border-radius: var(--shape-sm);
}

.tc-value {
  font-family: var(--font-mono, monospace);
  font-size: 1.1em;
  font-weight: 600;
  letter-spacing: 0.02em;
  background: var(--bg-surface);
  padding: var(--space-4) var(--space-5);
  border-radius: var(--shape-lg);
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.1em;
}

.tc-segment {
  position: relative;
}

.tc-segment::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--seg-color);
  border-radius: 2px;
  opacity: 0.5;
}

.tc-separator {
  color: var(--fg-muted);
  opacity: 0.4;
  margin: 0 0.1em;
}

.tc-legend {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--space-4);
}

.tc-legend-item {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
}

.tc-legend-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.tc-legend-name {
  font-family: var(--font-mono, monospace);
  font-size: 0.72em;
  font-weight: 700;
}

.tc-legend-desc {
  font-size: 0.72em;
  color: var(--fg-muted);
  line-height: 1.4;
}
</style>
