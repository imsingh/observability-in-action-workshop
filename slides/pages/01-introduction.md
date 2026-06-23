---
layout: section
number: 1
chip: Section 01
---

# Introduction to Modern Observability

What observability means and why it matters today

---
layout: default
heading: A Simple System
---

```mermaid {scale: 1.8}
flowchart LR
  U([User]) --> S([Server])
```

---
layout: default
heading: It Gets More Complex
---

```mermaid {scale: 1.4}
flowchart LR
  U([User]) --> LB([Load Balancer])
  LB --> S1([Service Instance 1])
  LB --> S2([Service Instance 2])
  S1 --> DB([DB])
  S2 --> DB
```

---
layout: default
heading: Even More Complex
---

```mermaid {scale: 1.4}
flowchart LR
  U([User]) --> LB([Load Balancer])
  LB --> S1([Service Instance 1])
  LB --> S2([Service Instance 2])
  S1 --> DB([DB])
  S2 --> DB
  S1 --> H([Infra / Host])
  S2 --> H
```

---
layout: statement
---

Observability is the ability to **measure a system's current state** based on the data it generates, recorded as logs, metrics, and traces
