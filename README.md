# MindBreak

AI 工作健康提醒 Skill。在你使用 Claude Code 持续工作时，自然地在回复末尾插入休息提醒。

## 安装

### 1. 放置 Skill 文件

将 `mindbreak/` 目录放到 `~/.claude/skills/` 下：

```
~/.claude/skills/mindbreak/
├── SKILL.md
└── README.md
```

### 2. 配置 Hook

在 `~/.claude/settings.json` 中添加 hook，让每次发消息时记录时间戳：

```json
{
  "hooks": {
    "user-prompt-submit": [
      {
        "command": "bash -c \"echo $(date +%s) >> ~/.claude/mindbreak_activity.log && cutoff=$(($(date +%s) - 86400)) && [ -f ~/.claude/mindbreak_activity.log ] && awk -v c=$cutoff '$1 >= c' ~/.claude/mindbreak_activity.log > ~/.claude/mindbreak_activity.tmp && mv ~/.claude/mindbreak_activity.tmp ~/.claude/mindbreak_activity.log\""
      }
    ]
  }
}
```

### 3. 验证

重启 Claude Code，正常工作一段时间后，检查 `~/.claude/mindbreak_activity.log` 是否有时间戳记录。

## 提醒类型

| 类型 | 触发条件 | 说明 |
|------|---------|------|
| 轻提醒 | 连续工作 > 45 分钟 | 建议起来走动 |
| 饭点提醒 | 11:30-12:30 / 17:30-18:30 | 附带任务小结 |
| 加班提醒 | 21:00 后仍在工作 | 建议收尾 |

## 控制

- 说"不用提醒"或"别提醒"可关闭当次会话的提醒
- 同一会话最多 3 次提醒
- 无 hook 时降级为基于对话轮数的粗略判断
