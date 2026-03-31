# Claude Code 类 Agent 产品重开发蓝图

## 目标

基于参考仓库提炼一个可自主实现的 agent 产品架构，不直接复刻原文件实现，而是抽象出最小可行系统与后续演进路径。

## 一句话定位

一个以 `CLI/REPL` 为入口、以 `工具调用` 为执行核心、以 `权限控制` 为安全边界、以 `扩展系统` 为能力增长机制的本地 agent 平台。

## 建议的系统分层

### 1. Interface Layer

职责：
- CLI 参数解析
- REPL 交互
- 消息展示
- 用户确认与权限弹窗

建议模块：
- `apps/cli`
- `ui/repl`
- `ui/components`

### 2. Orchestrator Layer

职责：
- 维护会话状态
- 调度模型调用
- 决定何时使用工具
- 管理对话、任务和 agent 生命周期

建议模块：
- `core/session`
- `core/orchestrator`
- `core/agents`

### 3. Tool Runtime Layer

职责：
- 统一定义工具接口
- 工具输入校验
- 工具权限判定
- 工具执行与结果归一化

首批工具建议只保留：
- `ReadFile`
- `SearchFile`
- `EditFile`
- `Bash`

暂缓实现：
- `WebFetch`
- `WebSearch`
- `MCP`
- `Plugin marketplace`
- `Remote bridge`

### 4. Security Layer

职责：
- 文件系统访问范围限制
- shell 命令白名单或半白名单
- 敏感路径拒绝
- 日志脱敏
- 凭据隔离

必须先做的安全能力：
- 工作区根目录限制
- 敏感路径 denylist
- shell 命令风险分类
- 明确的 `allow / ask / deny` 权限模型

### 5. Extension Layer

职责：
- 后续引入 skills、plugins、MCP

建议顺序：
1. 本地 skills
2. 本地插件目录
3. MCP
4. 远程 marketplace

不要一开始就做：
- 自动插件安装
- OAuth 驱动的第三方接入
- 远程代码执行桥接

## 推荐的 MVP 范围

第一阶段只做：
- 单会话 CLI
- 模型调用
- 文件读写
- shell 执行
- 基础权限系统
- 会话存档

第二阶段再做：
- 多 agent
- 任务列表
- diff 展示
- 项目级记忆

第三阶段再做：
- skills
- 本地插件
- MCP

第四阶段再做：
- 远程会话
- 团队同步
- 设置同步

## 核心数据流

1. 用户输入进入 REPL
2. Orchestrator 构造模型上下文
3. 模型返回普通消息或工具调用请求
4. Tool Runtime 执行工具
5. Security Layer 决定 `allow / ask / deny`
6. 结果写回会话状态
7. REPL 渲染结果并等待下一轮输入

## 你现在最值得学习的设计点

- 为什么工具系统要统一 schema 和权限判定
- 为什么 REPL 状态和工具执行状态要分离
- 为什么插件、skills、MCP 必须晚于核心工具层实现
- 为什么同步、桥接、遥测会显著扩大 trust boundary
- 为什么 agent 产品的安全问题主要来自“组合能力”

## 不建议直接继承的设计面

- 一开始就做完整插件市场
- 一开始就做 OAuth 与远程 bridge
- 一开始就做 team memory / settings sync
- 一开始就做全量多 agent 编排

## 推荐的新仓库目录草案

```text
my-agent/
  apps/
    cli/
  packages/
    core/
      orchestrator/
      session/
      tools/
      permissions/
    ui/
      repl/
    integrations/
      model/
    storage/
      sessions/
      config/
```

## 下一步实现顺序

1. 先定义 `Tool` 抽象和权限模型
2. 再做最小 REPL 和单轮对话
3. 接入 `ReadFile` 与 `Bash`
4. 做会话存档和 diff
5. 再考虑多 agent 和扩展机制
