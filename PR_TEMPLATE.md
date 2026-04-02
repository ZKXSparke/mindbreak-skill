# PR for daymade/claude-code-skills

## Title
feat: add mindbreak - AI work health reminder skill

## Description

### What does this skill do?
MindBreak monitors your continuous work time during Claude Code sessions and inserts natural break reminders at the end of responses. It tracks user activity through a hook-based timestamp system, calculates actual working duration (excluding breaks), and triggers three types of reminders: light break (45min+), meal time (11:30-12:30 / 17:30-18:30), and overtime (21:00+).

### Why is it useful?
AI coding tools create an "achievement addiction loop" — faster output → dopamine hit → want to do more → keep going. Developers using AI often skip meals, forget to stand up, and burn out without noticing. MindBreak acts as a caring friend who gently reminds you to take care of yourself.

### Key features
- **Precise tracking**: Hook records timestamps on every message; gaps > 15min = user was away, timer resets
- **Non-intrusive**: Reminders are appended naturally at the end of responses, never interrupting workflow
- **Smart**: Won't remind right after you come back from a break; max 3 reminders per session
- **Meal/overtime awareness**: Includes task summary so you can safely step away

### How to activate
The skill activates in any work-related conversation (coding, writing, analysis, design, debugging, research, planning).

### Testing
- Tested with simulated 50-minute continuous sessions
- Tested meal-time triggers at 11:30-12:30
- Tested gap detection (15min+ breaks correctly reset the timer)
- Tested "don't remind" user command

### Repository
https://github.com/ZKXSparke/mindbreak-skill

---

# PR for mhattingpete/claude-skills-marketplace

## Title
Add MindBreak: AI work health reminder skill

## Body
**MindBreak** — An AI work health reminder skill for Claude Code.

Monitors continuous work time and gently reminds you to take breaks, eat meals, and stop overtime. Uses hook-based activity tracking for precise work session detection.

- Repo: https://github.com/ZKXSparke/mindbreak-skill
- 3 reminder types: break / meal / overtime
- Smart gap detection: won't false-alarm after breaks
- Non-intrusive: natural language appended to response end
