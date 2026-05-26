# NovelForge Patterns

## Card System

Schema-driven content cards with JSON Schema validation.

## Workflow DSL

Code-based workflow definition (v2):
- Parse → Validate → Apply loop
- Node registration via @register_node decorator

## Event-Driven Triggers

```python
@on_event("card.saved")
def handle_card_saved(event: Event):
    # Check triggers, queue workflows
```

## Context Injection

@DSL syntax for referencing project data:
- @card[id] - Reference specific card
- @type[name] - Reference card type
- @relation[type] - Reference relations
