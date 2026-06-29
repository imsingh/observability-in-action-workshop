---
layout: section
number: 3
chip: Section 03
---

# Distributed Tracing

Following requests across services end-to-end

---
layout: default
heading: The Request Journey
clicks: 3
---

<div class="relative mt-2">
  <img v-if="$clicks === 0" :src="'/diagrams/trace-flow-p20-20.png'" class="max-h-[307px] object-contain mx-auto" />
  <img v-if="$clicks === 1" :src="'/diagrams/trace-flow-p21-21.png'" class="max-h-[307px] object-contain mx-auto" />
  <img v-if="$clicks === 2" :src="'/diagrams/trace-flow-p22-22.png'" class="max-h-[307px] object-contain mx-auto" />
  <img v-if="$clicks >= 3" :src="'/diagrams/trace-flow-p23-23.png'" class="max-h-[307px] object-contain mx-auto" />
</div>

---
layout: default
heading: Isolated Logs
---

<Stepper color="teal" class="mt-4">
  <StepperItem v-click title="Frontend / Device">
    <Stack gap="xs">
      <LogLine compact service="frontend" message="Page loaded" time="10:42:01.230" />
      <LogLine compact service="frontend" message="User scrolled to section" time="10:42:02.885" />
      <LogLine compact service="frontend" message="Button clicked, sending POST /api/order" time="10:42:03.001" />
      <LogLine compact service="frontend" message="Waiting for response..." time="10:42:03.004" level="warn" />
      <LogLine compact service="frontend" message="Another user clicked checkout" time="10:42:03.190" />
    </Stack>
  </StepperItem>
  <StepperItem v-click title="Load Balancer">
    <Stack gap="xs">
      <LogLine compact service="lb" message="Routing GET /api/products → service-b" time="10:42:02.100" />
      <LogLine compact service="lb" message="Routing POST /api/cart → service-a" time="10:42:02.740" />
      <LogLine compact service="lb" message="Routing POST /api/order → service-a" time="10:42:03.045" />
      <LogLine compact service="lb" message="Routing GET /api/user → service-b" time="10:42:03.120" />
      <LogLine compact service="lb" message="Routing DELETE /api/cart → service-a" time="10:42:03.210" />
    </Stack>
  </StepperItem>
  <StepperItem v-click title="Service A">
    <Stack gap="xs">
      <LogLine compact service="service-a" message="Cart updated for user 99" time="10:42:02.745" />
      <LogLine compact service="service-a" message="Processing order, querying DB" time="10:42:03.112" />
      <LogLine compact service="service-a" message="DB query took 84ms" time="10:42:03.196" level="warn" />
      <LogLine compact service="service-a" message="Order response sent" time="10:42:03.201" />
      <LogLine compact service="service-a" message="Cart cleared for user 99" time="10:42:03.215" />
    </Stack>
  </StepperItem>
  <StepperItem v-click title="Database">
    <Stack gap="xs">
      <LogLine compact service="db" message="UPDATE carts SET items=... WHERE user=99" time="10:42:02.748" />
      <LogLine compact service="db" message="SELECT * FROM products WHERE id IN (...)" time="10:42:02.990" />
      <LogLine compact service="db" message="SELECT * FROM orders WHERE id=42" time="10:42:03.198" />
      <LogLine compact service="db" message="INSERT INTO orders VALUES (...)" time="10:42:03.200" />
      <LogLine compact service="db" message="DELETE FROM carts WHERE user=99" time="10:42:03.216" level="warn" />
    </Stack>
  </StepperItem>
</Stepper>

---
layout: default
heading: Manual Correlation
---

<Stepper showAll class="mt-6">
  <StepperItem title="Open logs on the frontend" accent="teal">
    Find the log line for that request. Note the timestamp.
  </StepperItem>
  <StepperItem title="Switch to load balancer logs" accent="teal">
    Filter by timestamp range. Find the routing entry.
  </StepperItem>
  <StepperItem title="Jump to Service A or B logs" accent="teal">
    Search again. Hope the clocks are in sync.
  </StepperItem>
  <StepperItem title="Finally check the database logs" accent="teal">
    Correlate manually. This took you 20 minutes.
  </StepperItem>
</Stepper>

<Callout type="caution" title="The Problem" class="mt-6">
  Every subsystem logs in isolation. There is no shared context linking them together.
</Callout>

---
layout: statement
---

What if the request itself **carried a shared ID** — and every system just logged it?

---
layout: default
heading: One ID, Passed Along
---

<Timeline color="teal" class="mt-10">
  <TimelineItem v-click title="Frontend / Device">
    Generates <code>req-id: a1b2c3</code>, attaches to request
  </TimelineItem>
  <TimelineItem v-click title="Load Balancer">
    Reads <code>req-id: a1b2c3</code>, forwards it upstream
  </TimelineItem>
  <TimelineItem v-click title="Service A">
    Receives <code>req-id: a1b2c3</code>, logs it, passes to DB
  </TimelineItem>
  <TimelineItem v-click title="Database">
    Receives <code>req-id: a1b2c3</code>, logs it
  </TimelineItem>
</Timeline>

---
layout: default
heading: Now Logs Are Connected
---

<Stepper color="teal" showAll class="mt-4">
  <StepperItem title="Frontend / Device">
    <LogLine compact service="frontend" message="Button clicked, sending POST /api/order" time="10:42:03.001" request-id="a1b2c3" />
  </StepperItem>
  <StepperItem title="Load Balancer">
    <LogLine compact service="lb" message="Routing POST /api/order → service-a" time="10:42:03.045" request-id="a1b2c3" />
  </StepperItem>
  <StepperItem title="Service A">
    <LogLine compact service="service-a" message="Processing order, querying DB" time="10:42:03.112" request-id="a1b2c3" />
  </StepperItem>
  <StepperItem title="Database">
    <LogLine compact service="db" message="SELECT * FROM orders WHERE id=42" time="10:42:03.198" request-id="a1b2c3" />
  </StepperItem>
</Stepper>

<Callout v-click type="tip" title="Filter by req-id: a1b2c3 — instant full picture" class="mt-4" />

---
layout: statement
---

You just invented **distributed tracing**. The industry did too — and standardised it.

---
layout: default
heading: W3C Trace Context
---

<p class="mb-6 text-sm">Every HTTP request carries a <code>traceparent</code> header. Every system reads it, logs it, and passes it on.</p>

<TraceContext />

---
layout: statement
---

Tracing is a set of logs **chained together** via a **common request ID** — automatically connecting every hop a request makes through your system

---
layout: statement
---

One request. One trace ID. **Every subsystem.** No manual dot-connecting.

---
layout: default
heading: Exercise — Follow a Trace End-to-End
---

<Stepper showAll class="mt-4">
  <StepperItem title="Open your browser DevTools" accent="teal">
    Load the app, open the <strong>Network</strong> tab, and trigger a request (e.g. place an order).
  </StepperItem>
  <StepperItem title="Find the traceparent header" accent="teal">
    Select one request and inspect its <strong>Request Headers</strong>. Locate <code>traceparent</code> — it looks like <code>00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01</code>.
  </StepperItem>
  <StepperItem title="Extract the trace ID" accent="teal">
    The trace ID is the <strong>second segment</strong> — the 32-character hex string between the version and the parent ID. Copy it.
  </StepperItem>
  <StepperItem title="Open Distributed Traces in Dynatrace" accent="teal">
    Navigate to the <strong>Distributed Traces</strong> app and paste your trace ID into the filter to find the exact trace.
  </StepperItem>
  <StepperItem title="Investigate the trace" accent="teal">
    Open the waterfall view. Follow the request across every hop — which service was slowest? Where was time spent? Was there an error?
  </StepperItem>
</Stepper>
