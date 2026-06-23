<script setup lang="ts">
defineProps<{
  time?: string
  level?: 'info' | 'warn' | 'error'
  service: string
  message: string
  compact?: boolean
  requestId?: string
}>()

const levelColor: Record<string, string> = {
  info:  'var(--turquoise-700)',
  warn:  '#d97706',
  error: 'var(--maroon-800)',
}
</script>

<template>
  <div class="log-line" :class="{ 'log-line--compact': compact }">
    <span class="log-time">{{ time ?? '10:42:03.112' }}</span>
    <span class="log-level" :style="{ color: levelColor[level ?? 'info'] }">{{ (level ?? 'info').toUpperCase() }}</span>
    <span class="log-service">{{ service }}</span>
    <span class="log-message">{{ message }}</span>
    <span v-if="requestId" class="log-request-id">req=<strong>{{ requestId }}</strong></span>
  </div>
</template>

<style scoped>
.log-line {
  font-family: var(--font-mono, monospace);
  font-size: 0.72em;
  line-height: 1.8;
  display: flex;
  flex-wrap: wrap;
  gap: 0.6em;
  padding: 0.4em 0.6em;
  border-radius: var(--shape-sm);
  background: var(--bg-surface);
  color: var(--fg-muted);
}

.log-line--compact {
  font-size: 0.62em;
  padding: 0.25em 0.5em;
  gap: 0.4em;
  line-height: 1.6;
}

.log-time    { color: var(--fg-muted); opacity: 0.6; }
.log-level   { font-weight: 700; min-width: 3em; }
.log-service { color: var(--turquoise-500); font-weight: 600; }
.log-message { flex: 1; color: var(--fg-default); }
.log-request-id { color: var(--fg-muted); opacity: 0.8; }
.log-request-id strong { color: var(--turquoise-400); font-weight: 700; opacity: 1; }
</style>
