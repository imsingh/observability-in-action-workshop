---
layout: section
number: 4
chip: Section 04
---

# Intelligent Alerting

From thresholds to adaptive detection

---
layout: statement
---

An alert is a promise — **something is wrong, act now**. A broken promise is noise.

---
layout: default
heading: SLOs — What Are You Protecting?
---

<Stepper color="teal" showAll class="mt-4">
  <StepperItem title="Pick a metric that reflects user experience — SLI">
    <span class="text-xs">Example: <code>checkout success rate over the last 30 days = 99.82%</code>. Not CPU. Not memory. The thing users actually feel.</span>
  </StepperItem>
  <StepperItem title="Set a target — SLO">
    <span class="text-xs">Promise yourself a floor: <code>success rate &gt;= 99.9%</code>. Below that line, users notice and trust erodes.</span>
  </StepperItem>
  <StepperItem title="The target gives you a budget — Error budget">
    <span class="text-xs">99.9% availability = <strong>43 minutes</strong> of allowed downtime per month. Every incident burns some of that budget.</span>
  </StepperItem>
</Stepper>

<Callout v-click type="tip" title="The one question every alert should answer" class="mt-4">
  <span class="text-xs">Is this incident burning my error budget <strong>faster than I can afford?</strong> If yes — wake someone up. If no — log it and move on.</span>
</Callout>

---
layout: statement
---

So how do we know when an SLO is at risk? We set a **threshold** — and fire when the signal crosses it.

---
layout: default
heading: Static Thresholds
---

<Grid :cols="2" gap="md" class="mt-2">
  <Card variant="filled" padding="sm">
    <div class="font-semibold text-sm mb-1">How they work</div>
    <p class="text-xs">A fixed value you define. If the metric crosses it — fire the alert.</p>
    <Divider variant="subtle" />
    <div class="font-semibold text-sm mb-1">Common examples</div>
    <Stack gap="xs" class="text-xs">
      <div><code>CPU usage &gt; 80%</code></div>
      <div><code>Error rate &gt; 5%</code></div>
      <div><code>Response time &gt; 500ms</code></div>
    </Stack>
  </Card>
  <Stack gap="sm">
    <Card v-click variant="tonal" padding="sm">
      <div class="flex items-center gap-2 mb-1">
        <span class="i-ph-check-circle-bold w-4 h-4 text-current" />
        <span class="font-semibold text-xs">Strengths</span>
      </div>
      <Stack gap="xs" class="text-xs">
        <div>Simple to understand and configure</div>
        <div>No historical data required</div>
        <div>Predictable and auditable</div>
      </Stack>
    </Card>
    <Card v-click variant="tonal" padding="sm">
      <div class="flex items-center gap-2 mb-1">
        <span class="i-ph-x-circle-bold w-4 h-4 text-current" />
        <span class="font-semibold text-xs">Weaknesses</span>
      </div>
      <Stack gap="xs" class="text-xs">
        <div>One value cannot fit all traffic patterns</div>
        <div>Triggers on harmless spikes — alert fatigue</div>
        <div>Misses slow degradations that stay below the line</div>
      </Stack>
    </Card>
  </Stack>
</Grid>

---
layout: default
heading: Sliding Windows
---

<p class="text-xs mb-2">Instead of a single instant, a sliding window evaluates a <strong>rolling period of time</strong>. The alert fires when the average across that window crosses the threshold.</p>

<Stepper color="teal" showAll>
  <StepperItem title="Point-in-time check">
    <span class="text-xs">CPU at 90% for one second — alert fires immediately. A batch job ran. No real problem.</span>
  </StepperItem>
  <StepperItem title="5-minute sliding window">
    <span class="text-xs">CPU must average above 80% over five full minutes to fire. Brief spikes are absorbed.</span>
  </StepperItem>
  <StepperItem title="Choosing the window size">
    <span class="text-xs">Short windows (1–5 min) react fast but are noisier. Long windows (15–60 min) are stable but slow to catch real issues.</span>
  </StepperItem>
</Stepper>

<Callout v-click type="tip" title="Match the window to your SLO" class="mt-2">
  <span class="text-xs">A 99.9% SLO allows ~43 min downtime per month. A 5-minute window catches incidents before they breach it.</span>
</Callout>

---
layout: statement
---

What if the threshold itself learned what **normal** looks like — for every hour, every day of the week?

---
layout: default
heading: Baseline and Adaptive Thresholds
---

<Grid :cols="2" gap="md" rowGap="sm" align="stretch" class="mt-2 auto-rows-fr h-[310px]">
  <Card variant="filled" padding="sm">
    <div class="font-semibold text-sm mb-1">How it works</div>
    <p class="text-xs">The system observes historical data across hours, days, and weeks, then builds a model of expected behaviour. Alerts fire when the signal deviates beyond a confidence band.</p>
  </Card>
  <Card v-click="2" variant="tonal" padding="sm">
    <div class="flex items-center gap-2 mb-1">
      <span class="i-ph-warning-bold w-4 h-4 text-current" />
      <span class="font-semibold text-xs">Trade-offs</span>
    </div>
    <Stack gap="xs" class="text-xs">
      <div>Requires weeks of data to train reliably</div>
      <div>Harder to explain why an alert fired</div>
      <div>Can learn the wrong baseline if the system was already degraded</div>
    </Stack>
  </Card>
  <Card v-click="1" variant="tonal" padding="sm">
    <div class="font-semibold text-sm mb-1">Why it matters</div>
    <Stack gap="xs" class="text-xs">
      <div>Traffic is lower at 3 AM — the threshold adjusts automatically</div>
      <div>Monday morning spikes are expected — no false alert</div>
      <div>A real anomaly at any hour still triggers</div>
    </Stack>
  </Card>
  <Card v-click="3" variant="tonal" padding="sm">
    <div class="flex items-center gap-2 mb-1">
      <span class="i-ph-lightbulb-bold w-4 h-4 text-current" />
      <span class="font-semibold text-xs">Best used for</span>
    </div>
    <Stack gap="xs" class="text-xs">
      <div>Business metrics with clear daily or weekly patterns</div>
      <div>Error rates where an absolute threshold is hard to define</div>
      <div>Complementing static thresholds, not replacing them</div>
    </Stack>
  </Card>
</Grid>

---
layout: default
heading: Alert Fatigue
---

<Stepper showAll class="mt-6">
  <StepperItem title="Too many alerts fire" accent="teal">
    Engineers learn to ignore them. The one real incident gets missed.
  </StepperItem>
  <StepperItem title="Alerts without context" accent="teal">
    "CPU is at 81%" tells you nothing about user impact. Engineers investigate, find nothing wrong, and close the ticket.
  </StepperItem>
  <StepperItem title="No runbook attached" accent="teal">
    The on-call engineer wakes at 3 AM and does not know what to check first.
  </StepperItem>
</Stepper>

<Callout type="caution" title="The cost of noise" class="mt-6">
  Alert fatigue is the leading cause of missed incidents. Every false positive trains engineers to trust alerts less.
</Callout>

---
layout: default
heading: What to Alert On
---

<Grid :cols="2" gap="md" class="mt-2">
  <Card variant="filled" padding="sm">
    <div class="flex items-center gap-2 mb-1">
      <span class="i-ph-user-bold w-4 h-4 text-current" />
      <span class="font-semibold text-sm">Symptom-based — alert on these</span>
    </div>
    <Stack gap="xs" class="text-xs">
      <div>Users are experiencing errors — <code>error rate &gt; 1%</code></div>
      <div>Requests are slow — <code>p99 latency &gt; 1s</code></div>
      <div>Service is unavailable — <code>success rate &lt; 99.9%</code></div>
    </Stack>
  </Card>
  <Card variant="filled" padding="sm">
    <div class="flex items-center gap-2 mb-1">
      <span class="i-ph-gear-bold w-4 h-4 text-current" />
      <span class="font-semibold text-sm">Cause-based — investigate, do not page</span>
    </div>
    <Stack gap="xs" class="text-xs">
      <div>CPU high — may or may not affect users</div>
      <div>Memory growing — watch it, act when users feel it</div>
      <div>Disk usage at 70% — put it on a dashboard, not a pager</div>
    </Stack>
  </Card>
</Grid>

<Callout v-click type="tip" title="The RED method" class="mt-2">
  Alert on <strong>R</strong>ate (requests per second), <strong>E</strong>rrors (failure percentage), and <strong>D</strong>uration (latency) — these three directly reflect user experience.
</Callout>

---
layout: statement
---

Good alerting is not about catching every signal — it is about **waking the right person, for the right reason, at the right time**

---
layout: default
heading: Exercise — Your First Alert
---

<Stepper showAll class="mt-4">
  <StepperItem title="Open Dynatrace" accent="teal">
    Navigate to <strong>Alerts &amp; Anomaly Detection</strong> in your environment.
  </StepperItem>
  <StepperItem title="Create a metric alert" accent="teal">
    Search for a request duration metric for your service and select it. Set the aggregation to <strong>p99</strong>.
  </StepperItem>
  <StepperItem title="Configure a sliding window" accent="teal">
    Set the evaluation window to <strong>5 minutes</strong> and the threshold to <strong>500ms</strong>.
  </StepperItem>
  <StepperItem title="Generate load and observe" accent="teal">
    Run the load script from your terminal. Watch the chart — does the alert fire? Adjust the threshold until it does.
  </StepperItem>
  <StepperItem title="Add a workflow" accent="teal">
    Open <strong>Workflows</strong> and create a new workflow triggered by your alert. Add a <strong>Send email</strong> action and set the recipient to your own address. Save and trigger it manually to confirm delivery.
  </StepperItem>
</Stepper>
