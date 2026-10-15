# agent-tools-pack — AI 编程工具/技能精选包

> 整理自 2026-10-15 的 GitHub 周趋势榜（Star 增速 Top 10 筛掉无关项后的 AI 精选）。
> 目标读者：另一台机器上的 Claude Code，以及任何支持 SKILL.md 技能格式的 AI 编程 Agent。
> 整理机器：GoldAgent（Windows），原始克隆在 `F:/OK-WUWA/trending/`。

## 一图速览

| 本包目录 | 原仓库 | 许可 | 一句话价值 | 建议 |
|---|---|---|---|---|
| `skills/mattpocock-skills` | [mattpocock/skills](https://github.com/mattpocock/skills) | MIT | 38 个真实工程师技能包（本周 +11.9k★） | ⭐ 直接挂载 |
| `skills/diagram-design` | [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | MIT | 44+ 种编辑级图示设计规范（本周 +9.9k★） | ⭐ 直接挂载 |
| `claude-mem-notes.md` | [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | Apache-2.0 | Agent 跨会话持久记忆（本周 +3.6k★） | 🔧 按需安装 |

## 1. mattpocock-skills — 38 个工程师技能

作者是 Total TypeScript 的 Matt Pocock，把自己 `.agents` 目录里在用的技能包开源，
SKILL.md 标准格式，Claude Code 直接兼容。

- **engineering/（20 个）**：code-review（双轴评审）、diagnosing-bugs（诊断纪律）、
  tdd（红绿循环）、triage、to-spec、to-tickets、implement、prototype、research、
  retro、pr、domain-modeling、codebase-design、improve-codebase-architecture、
  grill-with-docs、wayfinder、wizard 等
- **productivity/（7 个）**：handoff（会话交接）、teach、grill-me、writing-for-agents 等
- **misc/（4 个）**：git-guardrails、setup-pre-commit 等
- **in-progress/（7 个）**：作者还在打磨的（chief-of-staff、claude-handoff 等）

**Claude Code 安装**：把想用的技能目录整体复制，例如
`skills/engineering/code-review/` → `~/.claude/skills/code-review/`（全局）
或项目 `.claude/skills/code-review/`（项目级）。
技能内引用的 GLOSSARY.md、tests.md 等同级文件记得一起拷。

**建议先试**：`code-review`（规范+规格双轴并行评审）、`diagnosing-bugs`（先建反馈
回路再动手的诊断纪律）、`tdd`、`handoff`（把当前会话压缩成交接文档）。

## 2. diagram-design — 编辑级图示设计

给 AI 编程工具用的图示设计规范：架构图（含 delta/containment 变体）、时序、ER、
甘特、桑基、热力、Wardley 地图、轴测图等 44+ 类型，纯 HTML/SVG 输出（明确不要
Mermaid）。`skills/diagram-design/assets/` 里有 218 个成品样例，先看效果再决定。

**Claude Code 安装**：`skills/diagram-design/` 整目录 → `.claude/skills/diagram-design/`。
首次出图它会要求定制样式（品牌色/网站/设计系统），不会默默用默认皮。

## 3. claude-mem — 持久记忆（源码未入包，见 claude-mem-notes.md）

跨会话持久记忆：记录 Agent 行为 → AI 压缩 → 相关上下文回注未来会话。
原生支持 Claude Code / OpenClaw / Codex / Gemini / Copilot 等宿主。
完整源码 300MB 没搬（另一台机器 `npm install -g claude-mem` 即可，
npm 官方源实测可解析 13.39.0），评估要点与本机接入现状见 `claude-mem-notes.md`。

## 整理机器上已完成的事（2026-10-15）

- 两个技能包已安装进 GoldAgent（`resources/skills/`）
- claude-mem 已作为 OpenClaw 插件注册，worker 服务运行中（端口 37777，开机自启）
- 附带产出：[HxhIcon/wuwa-video-route](https://github.com/HxhIcon/wuwa-video-route)
  阶段 2 完成（圆形掩码匹配 + 旋转搜索 + 端到端合成测试，4 个提交）

## 未收录的周趋势项目（与 AI 编程无关或价值存疑）

AnyPS5（PS5 移植，事件性爆红）、flutter（跨端 UI）、EpicGames/raddebugger（C++ 调试器）、
tester-army/e2e（有 Web 项目再看）、cursor/plugins（Cursor 生态）、
earthtojake/text-to-cad（制造业可围观）、heygen-com/hyperframes（HTML 渲染视频，做内容可看）。
