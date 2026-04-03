# rock-this-shut

A Claude Code skill that transposes the visual language of a reference image or GIF into a new use case — producing a technical spec and an approved visual mock ready for development.

---

## What it does

Most design tools help you generate interfaces from scratch or audit existing ones. **rock-this-shut** does something different: it takes a visual reference you already love — a dashboard, a game HUD, a military radar screen, an old terminal UI — and helps you adapt that exact visual language to a completely new purpose.

The process has two parallel tracks running together:

- **Visual track** — extracts colors, typography, spacing, animations, composition
- **Semantic track** — understands the *meaning* of each component and maps it to your new use case

You don't just get a style clone. You get a reasoned transposition: "this rotating orbital becomes your team cycle, these blinking counters become your KPIs."

### Example

You share a screenshot of an old military radar tracking system (black background, green monospace text, rotating orbital dots, numeric readouts). You tell Claude: "I want to use this to track my engineering team's project status."

The skill walks through:
1. Breaking down the radar into components (orbit, dots, numbers, title)
2. Understanding what the radar *means* as a system
3. Mapping each component to your team tracker (orbit → sprint cycle, dots → team members, numbers → KPIs)
4. Specifying the technical implementation
5. Generating a working HTML mock you can iterate on

You end the session with a spec document and a development plan — ready to hand off to your dev workflow.

---

## Phases

| Phase | What happens |
|-------|-------------|
| **DECODE** | Claude decomposes the reference into named components. You can add, edit, or remove any. Then you have a free conversation about what the reference *is* and *means* as a whole. |
| **ADAPT** | You describe your new use case. Claude maps each component to it — one at a time, with alternatives. Components that don't map cleanly are flagged explicitly. |
| **SPECIFY** | Claude writes the technical spec: colors, typography, animations, layout. Decides which tools/libraries to use, with justification. Does a web search if needed. |
| **MOCK** | Claude generates a self-contained HTML mock. You iterate until it looks right, then approve. |

Each phase ends with an explicit gate — Claude never advances without your confirmation.

**Output:** Two files written to your project when you approve the mock:
- `docs/rock-this-shut/rock-this-shut-spec.md` — full visual and technical specification
- `docs/rock-this-shut/rock-this-shut-plan.md` — development plan ready for your dev workflow

The skill does **not** write production code. It hands off to your normal development process.

---

## Installation

### Requirements
- [Claude Code](https://claude.ai/code) (CLI, desktop app, or IDE extension)

### Install

Copy the skill file to your Claude skills directory:

**macOS / Linux:**
```bash
mkdir -p ~/.claude/skills
curl -o ~/.claude/skills/rock-this-shut.md \
  https://raw.githubusercontent.com/ElJaimeDuende/rock-this-shut/main/rock-this-shut.md
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/ElJaimeDuende/rock-this-shut/main/rock-this-shut.md" `
  -OutFile "$env:USERPROFILE\.claude\skills\rock-this-shut.md"
```

**Manual:**
Download `rock-this-shut.md` from this repo and place it in:
- macOS/Linux: `~/.claude/skills/rock-this-shut.md`
- Windows: `%USERPROFILE%\.claude\skills\rock-this-shut.md`

---

## Usage

In any Claude Code session, invoke the skill:

```
rock-this-shut
```

Claude will ask you to share your reference image or GIF. From there, follow the phases.

### Tips

- Works with static images and animated GIFs
- Works in any language — Claude responds in whatever language you write in
- If your reference has an ambiguous meaning, Claude will offer 2-3 interpretations before proceeding
- You control the component list in DECODE — add, edit, or remove anything
- The holistic conversation in DECODE is where the magic happens: the more context you give about the origin (industry, era, purpose), the better the transposition

---

## Built with

This skill was designed and built using [Claude Code](https://claude.ai/code) with the [Superpowers skills system](https://github.com/anthropics/claude-code).

The workflow used:
- `superpowers:brainstorming` — designed the skill architecture
- `superpowers:writing-plans` — created the implementation plan
- `superpowers:subagent-driven-development` — built the skill task by task with spec compliance review at each step

---

## License

MIT
