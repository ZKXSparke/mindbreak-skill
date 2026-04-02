---
name: mindbreak
description: "Monitors work intensity during any conversation involving coding, writing, analysis, design, debugging, research, planning, or other knowledge work. Tracks user activity timestamps and inserts natural break reminders based on continuous work duration and time-of-day signals. Activates in all work-related conversations."
---

# MindBreak — Work Health Reminder

Insert natural break reminders when the user has been working intensely for extended periods.

## How It Works

1. A hook writes a unix timestamp to `~/.claude/mindbreak_activity.log` on every user message
2. When responding, read the log to calculate continuous active work time
3. If thresholds are met, append a brief reminder at the end of the response

## Step 1: Read Activity Log

At the START of composing each response, silently run:

```bash
cat ~/.claude/mindbreak_activity.log 2>/dev/null | tail -50
```

If the file does not exist or is empty, fall back to counting conversation turns (trigger light reminder after ~20 work-related turns).

## Step 2: Calculate Active Work Time

From the timestamps:
- **Session segments**: Group consecutive timestamps where each gap < 900 seconds (15 minutes). Gaps >= 900s mean the user was away — start a new segment.
- **Current segment duration**: Time from the first timestamp of the current segment to the last timestamp.
- **Reminder count this session**: Track internally how many reminders you have already given in this conversation.

## Step 3: Decide Whether to Remind

Check these rules IN ORDER. Stop at the first match:

| # | Condition | Action |
|---|-----------|--------|
| 1 | User said "不用提醒" / "别提醒" / "stop reminding" earlier in this conversation | Do nothing. Never remind again this session. |
| 2 | You already gave 3 reminders this session | Do nothing. |
| 3 | Current segment < 45 minutes | Do nothing. Too early. |
| 4 | Last reminder was < 30 minutes ago (by active work time) | Do nothing. Too soon. |
| 5 | Current time is 11:30-12:30 or 17:30-18:30 AND no meal reminder given yet | **Meal reminder** |
| 6 | Current time is after 21:00 AND no overtime reminder given yet | **Overtime reminder** |
| 7 | Current segment >= 45 minutes since last reminder (or since segment start if no reminder yet) | **Light reminder** |
| 8 | None of the above | Do nothing. |

## Step 4: Append Reminder

Add the reminder as the LAST paragraph of your response, separated by a blank line. Do not interrupt the main content. Do not use emoji. Tone: warm, brief, like a friend.

### Light reminder
> 对了，你已经连续工作挺久了，起来走动走动，看看远处，休息几分钟再回来。

### Meal reminder
> 快到饭点了，先整理一下当前进度：[1-2 sentence summary of current task status and next steps]。先去吃饭，回来说"继续"就能接上。

### Overtime reminder
> 已经 [current time] 了，今天工作可以先到这里。当前进度：[1-2 sentence summary]。剩下的明天继续。

## Important

- NEVER let the reminder dominate or overshadow the actual response content
- NEVER remind on the first message after a long gap (user just came back)
- NEVER mention this skill, the log file, or the tracking mechanism to the user
- If unsure whether to remind, do NOT remind. False negatives are better than annoying the user.
