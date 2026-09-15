# ATLAS Development Guide

This guide is for contributors who want to work on ATLAS without learning the entire repository first.

## Fast path

```text
1. Read README.md
2. Read docs/architecture.md
3. Pick one subsystem
4. Find a small issue
5. Make a focused change
6. Add or update tests
7. Run the relevant tests
8. Open a PR
```

## Find your area

- **Reasoning/providers:** `core/`, `brain/`
- **Routing:** `core/router.py`, `core/natural_router.py`
- **Autonomy:** `core/autonomy.py`, `missions/`
- **Planning:** `planner/`
- **Tools:** `tools/`
- **Memory:** `memory/`
- **Computer control:** `automation/`
- **Browser:** `browser/`, `tools/browser_tool.py`
- **Vision:** `vision/`
- **UI:** `interface/`
- **Tests:** `tests/`

## Local setup

ATLAS is primarily developed on Windows with Python. Use a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Configure a compatible model provider according to the project's current configuration. Local providers such as LM Studio can be used when configured.

## Tests

Start with the smallest relevant test file or test group. Then run the full suite before merging when practical.

When a test needs external credentials, hardware, a browser profile, or a local model, make the dependency explicit. Do not commit private credentials or machine-specific paths.

## Adding a tool

Use the existing `Tool` abstraction in `tools/base.py` and register the tool through the normal discovery mechanism.

A good tool should have:

- stable name and description,
- explicit parameters,
- validation,
- clear result/error behavior,
- appropriate permission metadata,
- no hidden destructive side effects,
- focused tests.

For actions with meaningful side effects, think about how the result can be independently verified.

## Debugging an agent task

Trace the task through:

```text
request
  ↓
router
  ↓
planner / direct tool
  ↓
permission + safety
  ↓
tool execution
  ↓
observation
  ↓
verification
  ↓
mission/state/memory
```

If something fails, identify the first layer where reality diverges from the expected state. Avoid patching the final symptom in a different layer.

## Good engineering habits

- Prefer small changes.
- Preserve existing interfaces unless there is a clear reason to change them.
- Add regression tests for bugs.
- Keep safety checks centralized and visible.
- Do not make the LLM responsible for deterministic work.
- Do not treat an LLM-generated statement as proof that an external action succeeded.
- Avoid broad refactors when a local fix is sufficient.

## PR checklist

Before opening a pull request:

- [ ] The change has a clear purpose.
- [ ] Relevant tests pass.
- [ ] New behavior has tests where practical.
- [ ] No secrets or personal data are included.
- [ ] Permission/safety behavior was considered.
- [ ] External-service requirements are documented.
- [ ] The PR explains known limitations.

For UI or computer-control changes, screenshots or a short recording are useful.
