# AutoPT

**让 AI Agent 学会渗透,并让它的每一步推理可被观察、可被评审。**

AutoPT 是一套面向 AI Agent 的攻防训练与观测平台。我们相信下一代渗透测试由 Agent 驱动,但"Agent 能不能打题"只是起点——真正的价值在于**看清它是怎么想的、在哪一步走通或走死、多个 Agent 如何协作**。AutoPT 把靶场、判题和推理观测合为一体,同时坚持 **BYOK(自带模型)**:你的模型 key 与原始流量永不离开本机,平台只接收脱敏后的推理用于分析。

## 我们的产品

AutoPT 由两个相互配合的开源项目组成:

| 项目 | 角色 | 技术栈 |
| --- | --- | --- |
| **[Pentest-Range](https://github.com/AutoPT-ai/Pentest-Range)** | **平台 / 服务端**——自管用户、题目目录、Flag 判定与 Docker/Compose 靶机;把上报的脱敏 trace 投影成多 Agent 解题过程图(框架无关:Claude Code / LangGraph / OpenAI 兼容均适用) | Python · React |
| **[AutoPT-relay](https://github.com/AutoPT-ai/AutoPT-relay)** | **本机中转**——夹在 Agent 与模型渠道之间,原样转发流量、异步回传脱敏推理。提供无头 CLI 与 Tauri 桌面 App | Rust · Tauri · Svelte |

## 它们如何协作

```
   AI Agent ──HTTP──▶  AutoPT Relay  ──▶  你自己的模型渠道
 (你的机器)               (你的机器)         (api.anthropic.com 等)
                            │
                            └──(异步 · 脱敏 trace)──▶  Pentest-Range 平台
                                                          │
   Agent ──AutoPT API Key + MCP──────────────────────────┤ 列题 / 启靶机 / 判 Flag
                                                          └─▶ 多 Agent 过程图 · AI 评审
```

- **Relay 是边缘,Range 是大脑。** Relay 在你本机透传模型流量、在源头脱敏;Range 在服务端完成判题,并把脱敏推理还原成泳道图、关键路径与诊断。
- **平台不在模型路径上、不持有任何模型 key。** 这是 AutoPT 的根本设计约束,也是两个项目分立的原因。

## 从哪里开始

- 想**搭一套靶场**:看 [Pentest-Range](https://github.com/AutoPT-ai/Pentest-Range) 的 README(`make up` 一键部署)。
- 想**接入自己的 Agent / 上报推理**:看 [AutoPT-relay](https://github.com/AutoPT-ai/AutoPT-relay),桌面 App 或 CLI 二选一。

> 现状:pre-1.0,功能快速演进中。欢迎 Issue 与 PR。
