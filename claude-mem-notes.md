# claude-mem 评估笔记（2026-10-15）

原仓库：<https://github.com/thedotmack/claude-mem>（Apache-2.0，v13.39.0，本周 +3.6k★）
完整源码在整理机 `F:/OK-WUWA/trending/claude-mem/`（300MB，未入本包）。

## 它是什么

跨会话持久记忆：记录 Agent 会话行为（observations）→ AI 压缩 → 相关上下文在
未来会话通过 `before_prompt_build` 钩子回注。原生支持 Claude Code、OpenClaw、
Codex、Gemini、Copilot 等宿主。核心组件是一个本地 worker 服务（默认端口 37777）。

## Claude Code 机器安装（另一台机器）

1. `npm install -g claude-mem`（npm 官方源实测可解析 13.39.0）
2. 按其 README 的 Claude Code 集成章节执行安装/挂钩
3. AI 压缩后端：默认读 Claude Code 凭据；也可在
   `~/.claude-mem/settings.json` 配 `CLAUDE_MEM_PROVIDER=openrouter/gemini`
   + 对应 API key（OpenRouter 有免费模型可用）

## 整理机（GoldAgent/OpenClaw 内核）接入现状

- 已以插件形式注册：`Runtime/Plugins/<runtime>/claude-mem`，官方 PluginCheck 通过
  （唯一警告是 `agent.prompt.inject` 高风险权限，属核心功能，加载时需显式授权）
- worker 已跑通：Bun 1.4.3 + 端口 37777，`status:ok / mcpReady:true`
- 计划任务 `GoldAgent-ClaudeMem-Worker` 开机自启（脚本在
  `Data/plugins/data/claude-mem/start-cm-worker.cmd`）
- 待办：AI provider 的 key 未配 → 当前 degraded（观察记录可用，压缩摘要不可用）

## 经验教训（给未来接入者）

- 官方 `openclaw/install.sh` 平台分支只有 macos/linux——别在 Windows 直接
  `curl | bash`；GoldAgent 的插件体系和原生 OpenClaw 目录布局不同，需手动接线
- 部署前先审脚本：worker 依赖 Bun ≥1.1.14 + `bun:sqlite`（Node 跑不了，
  Bun 用 `npm i -g bun` 装即可，走 npm registry 不需要碰被墙的 github 下载）
- 带掩码的匹配/压缩类工具注意 TM_CCORR_NORMED 的亮度敏感性
  （详见 wuwa-video-route 仓库 matching.py 的注释与测试）
