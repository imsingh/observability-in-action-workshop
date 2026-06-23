---
layout: section
number: 5
chip: Section 05
---

# Best Practices

Proven patterns for production observability

---
layout: default
heading: Instrument at the Right Level
---

<Grid :cols="2" gap="lg" class="mt-4">
  <Card variant="tonal">
    <div class="flex items-center gap-2 mb-3">
      <span class="i-ph-check-circle-bold w-5 h-5 text-current" />
      <span class="font-semibold">Do</span>
    </div>
    <Stack gap="xs" class="text-sm">
      <div>Instrument at service boundaries (HTTP, DB, queue)</div>
      <div>Capture errors and slow operations at 100%</div>
      <div>Use structured logs with consistent field names</div>
      <div>Propagate context across async boundaries</div>
      <div>Sample healthy, fast paths</div>
    </Stack>
  </Card>
  <Card variant="tonal">
    <div class="flex items-center gap-2 mb-3">
      <span class="i-ph-x-circle-bold w-5 h-5 text-current" />
      <span class="font-semibold">Avoid</span>
    </div>
    <Stack gap="xs" class="text-sm">
      <div>Logging inside tight loops</div>
      <div>Capturing request/response bodies by default</div>
      <div>High-cardinality span attribute values (e.g. user IDs)</div>
      <div>Ignoring health-check and synthetic traffic noise</div>
      <div>Adding observability as an afterthought</div>
    </Stack>
  </Card>
</Grid>

---
layout: default
heading: Sensitive Data in Telemetry
---

<Grid :cols="2" gap="lg" class="mt-4">
  <Card variant="filled">
    <div class="flex items-center gap-2 mb-3">
      <span class="i-ph-warning-bold w-5 h-5 text-current" />
      <span class="font-semibold">What leaks into telemetry</span>
    </div>
    <Stack gap="xs" class="text-sm">
      <div>User IDs, email addresses, names</div>
      <div>IP addresses and device fingerprints</div>
      <div>URL parameters with tokens or session data</div>
      <div>Request bodies with PII</div>
      <div>Error messages that echo user input</div>
    </Stack>
  </Card>
  <Card variant="filled">
    <div class="flex items-center gap-2 mb-3">
      <span class="i-ph-shield-check-bold w-5 h-5 text-current" />
      <span class="font-semibold">Mitigations</span>
    </div>
    <Stack gap="xs" class="text-sm">
      <div>Scrub or hash PII before emitting</div>
      <div>Use opaque IDs instead of real identifiers</div>
      <div>Mask URL query parameters in span attributes</div>
      <div>Apply attribute processors in your OTel collector</div>
      <div>Review what auto-instrumentation captures by default</div>
    </Stack>
  </Card>
</Grid>

<Callout type="caution" title="Your responsibility" class="mt-6">
  Observability tooling makes collection easy. That convenience does not transfer the legal or ethical responsibility away from the developer shipping the instrumentation.
</Callout>

---
layout: statement
---

Collect what you need to **understand your system** — not everything you *can* collect
