---
layout: section
number: 2
chip: Section 02
---

# Three Pillars Deep Dive

Logs, Metrics, and Traces in practice

---
layout: two-col
heading: The Analogy
split: 40/60
---

<template v-slot:left>
  <Stack gap="lg">
    <p>A single number rarely tells the whole story. Observability rests on three complementary signals.</p>
    <ul>
      <li v-click>Logs</li>
      <li v-click>Metrics</li>
      <li v-click>Traces</li>
    </ul>
  </Stack>
</template>

<template v-slot:right>
  <Youtube id="QFH64ToLyxk" width="100%" height="100%" />
</template>

---
layout: statement
---

**Logs** are the raw record of what happened — timestamped, contextual, human-readable events emitted by your system

---
layout: default
heading: Logs
---

<Grid :cols="2" gap="lg">
  <Card variant="filled">
    <div class="font-semibold mb-2">What they are</div>
    <p class="text-sm">Discrete, timestamped records of events. The closest thing to "your app talking to you".</p>
    <Divider variant="subtle" />
    <div class="font-semibold mb-2">When to use them</div>
    <p class="text-sm">Debugging specific errors, understanding what happened at a point in time, auditing.</p>
  </Card>
  <Stack gap="xs">
    <div class="text-xs font-semibold opacity-50 mb-1 uppercase tracking-wide">Example</div>
    <LogLine service="api" message="POST /api/order received" time="10:42:03.001" />
    <LogLine service="api" message="Validating order payload" time="10:42:03.004" />
    <LogLine service="api" message="DB query took 84ms" time="10:42:03.088" level="warn" />
    <LogLine service="api" message="Order 42 created successfully" time="10:42:03.201" />
    <LogLine service="api" message="Response sent 200 OK" time="10:42:03.205" />
  </Stack>
</Grid>

---
layout: default
heading: Log Structure
---

<Stepper>
  <StepperItem v-click title="Timestamp">
    When the event occurred — ISO 8601 or Unix epoch, always in UTC
  </StepperItem>
  <StepperItem v-click title="Severity">
    The importance level — DEBUG, INFO, WARN, ERROR, or FATAL
  </StepperItem>
  <StepperItem v-click title="Attributes">
    Key-value pairs that add context — user ID, request ID, region, service name
  </StepperItem>
  <StepperItem v-click title="Resources">
    Metadata about the source — the host, container, or process that emitted the log
  </StepperItem>
  <StepperItem v-click title="Body">
    The main content of the log — the human-readable message describing what happened
  </StepperItem>
</Stepper>

---
layout: statement
---

Open your terminal and run **`fetch`** — we'll look at real logs together

---
layout: statement
---

**Metrics** are numeric measurements over time — giving you the pulse of your system at a glance

---
layout: default
heading: Metrics
---

<Grid :cols="2" gap="lg">
  <Card variant="filled">
    <div class="font-semibold mb-2">What they are</div>
    <p class="text-sm">Aggregated numbers sampled over time. Cheap to store, fast to query, great for alerting.</p>
    <Divider variant="subtle" />
    <div class="font-semibold mb-2">When to use them</div>
    <p class="text-sm">Dashboards, SLOs, capacity planning, anomaly detection, alerting on thresholds.</p>
  </Card>
  <Stack gap="md">
    <div class="text-xs font-semibold opacity-50 uppercase tracking-wide">Common metrics</div>
    <Grid :cols="2" gap="md" rowGap="md">
      <Card v-click variant="tonal">
        <div class="text-xs opacity-60 mb-1">Request rate</div>
        <div class="text-2xl font-bold">2.4k <span class="text-sm font-normal opacity-60">req/s</span></div>
      </Card>
      <Card v-click variant="tonal">
        <div class="text-xs opacity-60 mb-1">Error rate</div>
        <div class="text-2xl font-bold text-amber-400">0.3<span class="text-sm font-normal opacity-60">%</span></div>
      </Card>
      <Card v-click variant="tonal">
        <div class="text-xs opacity-60 mb-1">Latency p99</div>
        <div class="text-2xl font-bold">184 <span class="text-sm font-normal opacity-60">ms</span></div>
      </Card>
      <Card v-click variant="tonal">
        <div class="text-xs opacity-60 mb-1">CPU usage</div>
        <div class="text-2xl font-bold text-amber-400">71<span class="text-sm font-normal opacity-60">%</span></div>
      </Card>
    </Grid>
  </Stack>
</Grid>

---
layout: statement
---

Time to see **metrics in action** — open Dynatrace and explore the dashboard

---
layout: statement
---

**Traces** show the full journey of a request — connecting every hop across every service into one picture

---
layout: default
heading: Traces
---

<Grid :cols="2" gap="lg">
  <Card variant="filled">
    <div class="font-semibold mb-2">What they are</div>
    <p class="text-sm">A chain of spans — each span is one hop in the request's journey, with timing and context.</p>
    <Divider variant="subtle" />
    <div class="font-semibold mb-2">When to use them</div>
    <p class="text-sm">Diagnosing latency, finding which service is slow, understanding cross-service dependencies.</p>
  </Card>
  <Stack gap="xs">
    <div class="text-xs font-semibold opacity-50 mb-1 uppercase tracking-wide">A single trace</div>
    <TraceContext />
  </Stack>
</Grid>

---
layout: statement
---

Time to see **traces in action** — open Dynatrace and follow a request end-to-end

---
layout: statement
---

No single pillar is enough. **Logs** tell you what, **metrics** tell you when, **traces** tell you where.
