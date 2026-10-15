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

## 用第三方 API 的 Claude Code（如 DeepSeek ds4.1flash）——2026-10-15 补充

场景：Claude Code 用 `ANTHROPIC_BASE_URL` + 环境变量 token 接 DeepSeek 等
第三方模型（非官方 OAuth 订阅）。逐项影响：

- **技能包（mattpocock / diagram-design）**：零功能影响。技能是纯 Markdown
  指令，任何模型都能加载；区别只在执行质量——flash 档跑流程型技能
  （code-review / triage / handoff）够用，重推理技能（架构分析 / 诊断 /
  domain-modeling）和 diagram-design 的出图质量会明显打折
- **claude-mem 压缩后端**：settings.json 里有专门的 OpenAI 兼容接入口——
  `CLAUDE_MEM_PROVIDER` 设 openai_compat（确切取值以实测为准），
  `CLAUDE_MEM_OPENAI_COMPAT_BASE_URL`（如 https://api.deepseek.com）、
  `CLAUDE_MEM_OPENAI_COMPAT_API_KEY`、`CLAUDE_MEM_OPENAI_COMPAT_MODEL`
  （填 ds4.1flash 的确切模型名）。压缩=总结类任务，flash 档正好够用
- **注意默认路径**：provider=claude + auth=subscription 读的是 Claude Code
  凭据存储里的 OAuth——环境变量方式接入的 Claude Code 大概率没有这份凭据，
  走上面的 openai_compat 配置即可
- **claude_cli 依赖**：claude-mem 会 spawn `claude` 可执行文件做观察，
  确保它在 PATH 或设 `CLAUDE_CODE_PATH`；它继承 Claude Code 的同一套路由
- **附带依赖**：本地向量库 Chroma 默认开启（uvx 拉起，机器要有 uv）；
  tier routing 里的 haiku/sonnet 别名在非 Anthropic 后端下的映射需实测确认
