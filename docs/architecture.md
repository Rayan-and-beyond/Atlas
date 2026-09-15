# ATLAS Architecture

ATLAS is organized as a set of cooperating subsystems rather than one monolithic assistant.

## Core loop

```text
UNDERSTAND
    ↓
ROUTE
    ↓
PLAN
    ↓
PERMISSION + SAFETY
    ↓
EXECUTE
    ↓
OBSERVE
    ↓
VERIFY
    ↓
REMEMBER
    ↓
CONTINUE / FINISH / RECOVER
```

The important design principle is that the LLM is **one component** of the agent, not the entire agent.

## Major layers

### Brain

The brain provides model intelligence and context handling. Providers should be replaceable so ATLAS can work with local or compatible remote models without coupling the rest of the system to one provider.

### Router

The router interprets a user request and decides which path should handle it: deterministic commands, memory, skills, tools, missions, or the general model path.

### Planner

Complex goals can become structured tasks. The planner is responsible for decomposition, execution order, retry/recovery behavior, and task-level verification.

### Permissions and hard safety

Actions are checked before execution. Hard safety rules take precedence over model intent. Confirmation-sensitive actions must not be silently approved by autonomous work.

### Tools

Tools are modular capabilities such as files, browser operations, system actions, computer automation, media, and integrations. Tools are discoverable and expose metadata describing their permissions and parameters.

### Observation and verification

ATLAS should distinguish between **an action being attempted** and **the requested outcome actually occurring**.

For example:

```text
click Send
   ↓
observe
   ↓
message appears in Sent
   ↓
verified
```

The long-term goal is tool-specific, evidence-based verification rather than relying only on return strings or exceptions.

### Memory and state

Persistent memory stores useful facts and context. Agent state, goals, plans, checkpoints, and experiences allow work to continue beyond a single interaction.

### Autonomy

The autonomy layer coordinates missions, checkpoints, self-evaluation, experience recording, and recovery. Persistent goals should remain bounded by permissions, safety rules, resource limits, and user intent.

## Project map

```text
core/        Routing, providers, permissions, safety, execution
planner/     Planning, task execution, verification, recovery
missions/    Persistent projects, goals, and tasks
tools/       Discoverable integrations and computer capabilities
skills/      Drop-in higher-level skills
memory/      Persistent memory and retrieval
automation/  Windows input, clipboard, accessibility, processes
browser/     Browser sessions and automation
vision/      Screen understanding and OCR/vision helpers
voice/       Voice configuration and audio pipeline
agents/      Specialized agent/delegation support
interface/   Desktop interface and settings
tests/       Automated tests
```

## Design rules

1. **Keep the brain replaceable.** Do not make core behavior depend on one model provider.
2. **Prefer deterministic code for deterministic work.** Use the model where interpretation or planning is actually needed.
3. **Check before acting.** Permissions and hard safety happen before meaningful side effects.
4. **Observe after acting.** An agent needs evidence about what changed.
5. **Verify outcomes.** A successful tool call is not necessarily a successful task.
6. **Recover deliberately.** Retry transient failures, change strategy when appropriate, and stop when blocked.
7. **Persist bounded work.** Goals and checkpoints should survive process restarts without becoming uncontrolled background behavior.
8. **Keep components replaceable.** New tools, providers, skills, and interfaces should not require rewriting the entire system.
9. **Test behavior, not just imports.** End-to-end paths matter more than whether a function can be called.
10. **Never create a god object.** No single file should own routing, tools, safety, memory, UI, and autonomy.

## Changing the architecture

Before moving responsibilities between layers, identify:

- who owns the state,
- where permissions are checked,
- what evidence proves success,
- how failures propagate,
- how the behavior is tested,
- and whether the change affects autonomous or destructive actions.

Small, explicit boundaries are preferred over large rewrites.