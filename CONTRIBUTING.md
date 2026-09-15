# Contributing to ATLAS 🤖

Thanks for wanting to help build ATLAS.

ATLAS is a local-first AI agent for Windows. You do **not** need to understand the entire codebase before contributing. Pick one subsystem, make a focused change, add or update tests, and explain what changed.

## Where to start

| Area | Location | Good contribution |
|---|---|---|
| Core routing | `core/` | routing bugs, cleaner dispatch, tests |
| Planning & autonomy | `planner/`, `missions/` | recovery, verification, mission behavior |
| Tools | `tools/` | new tools, reliability, validation |
| Memory | `memory/` | retrieval, ranking, persistence |
| Computer control | `automation/` | Windows interaction and reliability |
| Browser | `browser/`, `tools/browser_tool.py` | navigation, state, verification |
| Vision | `vision/` | OCR, screen understanding |
| UI | `interface/` | desktop UX and observability |
| Tests | `tests/` | regression and end-to-end coverage |
| Documentation | `README.md`, `docs/` | guides, examples, architecture docs |

## Before you start

1. Read the relevant module before changing it.
2. Check existing issues and pull requests so work is not duplicated.
3. Prefer small, focused changes over large rewrites.
4. Do not weaken safety or permission boundaries to make a test pass.
5. Never commit API keys, passwords, OAuth tokens, personal data, or local machine secrets.

## Development

ATLAS is primarily developed on Windows with Python. Create a virtual environment and install the project dependencies from `requirements.txt`.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Run the test suite before submitting a change. If a test depends on hardware, credentials, a browser session, or a local model, document that clearly rather than silently skipping behavior.

## Making a change

A good contribution usually looks like:

```text
Issue
  ↓
Understand the existing behavior
  ↓
Make the smallest useful change
  ↓
Add/update tests
  ↓
Run the relevant tests
  ↓
Run the full suite when practical
  ↓
Explain limitations in the PR
```

## Tool contributions

New tools should use the existing `Tool` abstraction and metadata system. Keep tool behavior deterministic where possible and make permissions explicit.

A tool should:

- Have a clear, stable name.
- Validate its arguments.
- Return useful success/failure information.
- Avoid hidden destructive behavior.
- Respect ATLAS permissions and hard-safety boundaries.
- Be testable without requiring a real user's private data.

## Agent behavior

When changing planning, autonomy, memory, routing, or execution, think in terms of the full loop:

```text
UNDERSTAND → ROUTE → PLAN → PERMISSION → EXECUTE
     ↑                                      ↓
 REMEMBER ← RECOVER ← VERIFY ← OBSERVE ←───┘
```

A successful function call is not automatically proof that the user's goal succeeded. Prefer observable evidence when adding verification.

## Pull requests

Keep pull requests focused. A useful PR description answers:

- What problem does this solve?
- What changed?
- How was it tested?
- What remains unfinished?
- Does this affect permissions, safety, privacy, or external services?

Screenshots or short recordings are welcome for UI and computer-control changes.

## Good first contributions

If you are new to ATLAS, documentation, tests, diagnostics, error messages, and small tool improvements are excellent places to start.

Look for GitHub issues labeled:

- `good first issue`
- `help wanted`
- `documentation`
- `testing`
- `ui`
- `tools`

## Code style

Follow the surrounding code instead of introducing a new style for one file. Avoid unnecessary rewrites. Clear names, small functions, explicit errors, and tests are preferred over clever abstractions.

## Safety

ATLAS can interact with a real Windows machine. Treat changes to automation, shell/process execution, files, credentials, browser sessions, and system configuration as security-sensitive.

If you discover a security vulnerability, do not publish working exploit details in a normal issue. Contact the project maintainer privately through the repository's available security/contact channel.

## The goal

ATLAS is meant to become a capable, inspectable, local-first computer agent. Contributions that make it **more reliable, more understandable, safer, and easier to extend** are especially valuable.

Thanks for helping build it. 🛠️