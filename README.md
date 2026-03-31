# Claude Code Leaked Source Code

这是一个用于参考分析的仓库，不作为直接运行、直接二次开发或自动修复的工作目录。

## 仓库定位

- `src/`：原始参考源码目录，按约束只读分析，不修改、不删除、不重命名。
- 仓库根目录：分析输出区域，用于放置说明文档、威胁模型、重开发蓝图、忽略规则等辅助文件。

## 仓库结构

```text
.
├── src/                         参考源码
├── AGENTS.md                    仓库分析与操作约束
├── README.md                    项目说明
├── claude-code-threat-model.md  威胁模型
├── redevelopment-blueprint.md   重开发蓝图
└── .gitignore                   Git 忽略规则
```

## 当前内容

### 1. 参考源码

- `src/`

从当前目录结构看，源码主要围绕以下能力展开：

- CLI / REPL 入口
- 命令系统
- 工具执行层
- 权限控制
- 插件与 skills
- MCP 与远程能力
- 会话、状态与服务层

### 2. 分析文档

- `claude-code-threat-model.md`
- `redevelopment-blueprint.md`

其中：

- `claude-code-threat-model.md`：聚焦本地文件读写、命令执行、插件、MCP、同步与遥测等风险面。
- `redevelopment-blueprint.md`：提炼一个可自主实现的 agent 产品分层与演进路线。

## 核心模块概览

基于当前 `src/` 结构，可以把系统大致理解为以下几层：

- `src/cli`、`src/screens`、`src/components`：CLI 与 REPL 交互层
- `src/commands`：命令分发与子命令实现
- `src/tools`：工具定义与执行能力
- `src/tasks`、`src/coordinator`、`src/state`：任务编排与状态管理
- `src/services`、`src/remote`、`src/bridge`：远程能力、同步与桥接
- `src/plugins`、`src/skills`、`src/services/mcp`：扩展系统与 MCP 集成
- `src/utils`：权限、认证、存储、日志、telemetry 等底层支撑

## 使用原则

1. 只把本仓库当作参考分析材料。
2. 不在 `src/` 内做源码修改。
3. 新增说明、整理、发布辅助文件时，统一放在 `src/` 外部。
4. 不直接逐文件复刻实现，优先提炼模块、数据流、权限模型与安全边界。

## 推荐阅读顺序

1. 先看 `src/` 的目录划分，理解模块边界。
2. 再看 `claude-code-threat-model.md`，识别高风险能力与信任边界。
3. 最后看 `redevelopment-blueprint.md`，将参考实现抽象为可自主实现的系统设计。

## 已识别的高风险能力

- 本地文件读写
- Shell / 命令执行
- 外网访问
- 插件扩展
- MCP 集成
- 同步与遥测

## 适合进一步补充的分析文档

- 模块调用关系图
- 工具权限模型拆解
- MCP / 插件 / 远程桥接的数据流图
- 命令系统与工具系统的时序图
- 面向重开发的新接口草案

## 维护约定

- 与源码直接相关的路径说明，统一使用 `src/`
- 规则、分析文档、README 统一放在仓库根目录
- 若目录结构再次调整，应同步更新 `AGENTS.md`、`README.md` 和分析文档中的路径引用

## 说明

本仓库的根目录文件主要用于分析、整理与发布辅助，不视为 `src/` 原始源码的一部分。
