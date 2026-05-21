# Activation Cases — urban-gardener-agent

> **Scope**: Should-trigger / should-not-trigger / near-miss phrases for routing validation.
> **Load when**: auditing routing quality or tuning the agent description.

---

## Should Trigger

| Phrase | Why This Agent |
|--------|---------------|
| "Plan my garden for next season" | Core domain: seasonal planning |
| "What vegetables can I grow on my balcony?" | Urban growing space + plant selection |
| "Create a planting schedule for tomatoes and peppers" | Schedule generation request |
| "I need to track my garden inventory — pots, soil, seeds" | Inventory management |
| "When should I start tomatoes indoors in the Netherlands?" | Zone-specific timing question |
| "Make a harvest calendar for my raised bed" | File generation + harvest planning |
| "What's the best treatment for aphids on my tomatoes?" | Pest treatment guidance |
| "Generate an Excel file for my garden plan" | Explicit Excel generation |
| "Help me plan a succession planting schedule for lettuce" | Succession + scheduling |

---

## Should NOT Trigger

| Phrase | Correct Route | Why Not This Agent |
|--------|--------------|-------------------|
| "I need a landscape design for my front yard" | general-purpose or ui-design-engineer | Landscape architecture, not growing management |
| "Set up a farm management system for 50 hectares" | general-purpose | Commercial scale, not urban/home |
| "Identify this plant disease from this photo" | general-purpose (vision) | Image analysis outside this agent's scope |
| "Design an irrigation system with sensors and automation" | nodejs-api-engineer or general-purpose | Engineering/IoT, not horticultural planning |
| "What's the weather forecast for Amsterdam next week?" | general-purpose | Weather data retrieval, not gardening |

---

## Near-Miss Phrases (Require Careful Routing)

| Phrase | Correct Route | Reasoning |
|--------|--------------|-----------|
| "Can you help me grow my business?" | general-purpose | "grow" as metaphor, not horticulture |
| "What plants look good in my living room?" | general-purpose | Interior decor, not growing/harvesting focus |
| "I want to start a community garden project" | general-purpose | Project management + social, not personal garden planning |
| "How do I propagate succulents from cuttings?" | This agent (borderline) | Plant propagation is within scope; load treatment-guide.md |
