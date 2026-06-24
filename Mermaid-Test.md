---
title: Mermaid Test
date: 2026-06-24
tags: 
  - md-test
---

# Mermaid Test

This page is meant to verify that Silverbullet renders Mermaid diagrams correctly.

If everything is working, each diagram below should render as a diagram instead of showing raw code fences.

## Flowchart

```mermaid
flowchart TD
  A[Start] --> B{Build passes?}
  B -->|No| C[Fix issue]
  C --> A
  B -->|Yes| D[Render page]
  D --> E[Inspect output]
```

## Sequence Diagram

```mermaid
sequenceDiagram
  participant U as User
  participant S as Silverbullet
  participant M as Mermaid

  U->>S: Open Mermaid-Test.md
  S->>M: Render mermaid block
  M-->>S: SVG output
  S-->>U: Display diagram
```

## Class Diagram

```mermaid
classDiagram
  class Page {
    +title
    +content
    +render()
  }
  class Renderer {
    +parse()
    +draw()
  }
  Page --> Renderer : uses
```

## State Diagram

```mermaid
stateDiagram-v2
  [*] --> Idle
  Idle --> Loading: open page
  Loading --> Ready: render succeeds
  Loading --> Error: render fails
  Error --> Idle: retry
  Ready --> [*]
```

## Large Graph

```mermaid
flowchart LR
  subgraph Ingestion
    A1[Fetch source]
    A2[Parse markdown]
    A3[Normalize blocks]
    A4[Cache tokens]
  end

  subgraph Rendering
    B1[Select renderer]
    B2[Build AST]
    B3[Resolve links]
    B4[Layout nodes]
  end

  subgraph Output
    C1[Paint HTML]
    C2[Inject styles]
    C3[Measure height]
    C4[Update viewport]
  end

  subgraph Checks
    D1{Syntax valid?}
    D2{Assets loaded?}
    D3{Layout stable?}
    D4{Interactive?}
  end

  A1 --> A2 --> A3 --> A4
  A4 --> B1
  B1 --> B2 --> B3 --> B4
  B4 --> C1 --> C2 --> C3 --> C4
  C4 --> D1
  D1 -->|No| A2
  D1 -->|Yes| D2
  D2 -->|No| B1
  D2 -->|Yes| D3
  D3 -->|No| B4
  D3 -->|Yes| D4
  D4 -->|No| C1
  D4 -->|Yes| Z[Done]

  A2 -.-> D1
  A3 -.-> D2
  B2 -.-> D3
  C2 -.-> D4
  C3 -.-> Z
```

## Dense Sequence

```mermaid
sequenceDiagram
  participant U as User
  participant E as Editor
  participant P as Parser
  participant R as Renderer
  participant L as Layout
  participant V as Viewport

  U->>E: Open page
  E->>P: Read Mermaid block
  P->>P: Validate syntax
  P->>R: Build diagram model
  R->>L: Calculate positions
  L->>V: Fit to page width
  V-->>R: Dimensions
  R-->>E: SVG markup
  E-->>U: Display diagram
  U->>E: Scroll and resize
  E->>L: Recompute height
  L-->>E: New layout size
```

## What to Check

- Flowchart nodes should have arrows and decision branches.
- Sequence diagram should show participants and message arrows.
- Class diagram should show labeled relationships.
- State diagram should show transitions and terminal states.
- The large graph should create more layout work and be visibly denser.
- The dense sequence should help reveal any slow redraw or sizing issues.
- If any block stays as plain text, Mermaid rendering is not working.
