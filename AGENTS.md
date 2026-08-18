# Smart Agent 项目指南

## 项目概述

Smart Agent 是一个智能体项目，用于构建和运行具备自主决策与任务执行能力的 AI 代理。
项目通过模块化架构，整合大语言模型（LLM）、工具调用（Tool Calling）与记忆机制，让代理能够理解用户意图并高效完成任务。

当前处于初始开发阶段。后续将逐步补充：
- 核心代码
- 配置说明
- 使用文档

## 工作约定

- 本项目当前为早期阶段，暂无可运行脚本。改动后请保持工作树整洁。
- 新增核心代码、配置或文档时，请更新 README 与本文档保持一致。
- 提交信息使用简洁、描述性的英文（e.g. `Update README with project description`）。
- 不要将 `node_modules/`（已在 .gitignore 中忽略）或其他依赖产物提交进仓库。
- 在上下文对话中，发现并使用简洁、描述性的中文（e.g. `用户倾向、喜欢使用xxx`）将用户的偏好加入工作目录下的AGENTS.md中的用户偏好部分。
- 在上下文对话中，发现并使用简洁、描述性的中文（e.g. `AI在xxx处踩过xxx问题的坑`）将AI踩过的坑加入工作目录下的AGENTS.md中的AI踩过的坑部分。

## 架构方向（规划）

本项目基于 **pi（@earendil-works/pi-coding-agent）** 扩展构建。pi 作为底层 harness，Smart Agent 通过 **扩展（extensions/插件）** 注入智能体能力，与 pi 共同构成执行框架。

### 扩展机制

- 项目本地扩展放在 `.pi/extensions/`（pi 自动发现加载），全局扩展放在 `~/.pi/agent/extensions/`。
- 扩展通过 `registerTool()` 注册 LLM 可调用工具、`on(event, handler)` 拦截事件、`registerCommand()` 添加命令、`registerProvider()` 注册模型、`ctx.ui` 与用户交互、`resources_discover` 贡献 skills/prompts/themes。
- 扩展使用 TypeScript，入口为导出默认工厂函数（接收 `ExtensionAPI`），由 jiti 加载无需编译。

### 模块划分（规划，对应扩展实现）

本项目将围绕以下模块展开，开发时请遵循模块化设计，每个模块对应独立的扩展文件或目录：

- **LLM 接入层**：负责与各类大语言模型交互、请求/响应处理。可利用 `registerProvider()` 动态注册模型提供方，或通过 `before_provider_request` / `after_provider_response` 等事件干预请求链路。
- **工具调用层**：注册与调度代理可调用的外部工具。通过 `registerTool()` 注册，可用 `tool_call` / `tool_result` 事件做拦截与结果改写。
- **记忆机制**：管理短期与长期记忆，支撑多轮任务的上下文。利用 `ctx.sessionManager` 读写会话状态持久化，可结合 AGENTS.md 中的「用户偏好」与「AI踩过的坑」两节。
- **任务执行与决策**：理解用户意图、规划步骤并自主执行。可借助 `before_agent_start` 注入上下文、`input` 事件路由、自定义命令等实现。

### 开发约定

- 新增扩展时，在 `.pi/extensions/` 下建立模块目录，遵循「单文件」或「目录+index.ts」两种风格。
- 涉及外部依赖时在扩展目录（或父级）添加 `package.json` 并 `npm install`。
- 扩展为智能体能力主体，与 pi 共同作为 harness 部分运行。

## 用户偏好
- 暂无

## AI踩过的坑
- 暂无
