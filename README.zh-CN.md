# EveryProxy：流量代理订购与凭据交付

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/hero-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/hero-light.svg">
    <img alt="一笔动态住宅代理订单获批、开通，凭据只交付一次，不写进共享日志。" src="assets/hero-light.svg" width="100%">
  </picture>
</p>

<p align="center">
  <a href="https://cursor.com/en/install-mcp?name=everyinfra&config=eyJ1cmwiOiJodHRwczovL2FwaS5ldmVyeWluZnJhLmNvbS9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgPEFQSSBLZXk%2BIn19"><img src="https://img.shields.io/badge/在_Cursor_安装-2F363D?style=for-the-badge&logo=cursor&logoColor=white" alt="在 Cursor 安装"></a>
  <a href="https://vscode.dev/redirect/mcp/install?name=everyinfra&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.everyinfra.com%2Fmcp%22%2C%22headers%22%3A%7B%22Authorization%22%3A%22Bearer%20%3CAPI%20Key%3E%22%7D%7D"><img src="https://img.shields.io/badge/在_VS_Code_安装-0065A9?style=for-the-badge&logo=githubcopilot&logoColor=white" alt="在 VS Code 安装"></a>
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/blob/main/i18n/README.zh-CN.md#claude-code"><img src="https://img.shields.io/badge/Claude_Code-一条命令-B4532A?style=for-the-badge&logo=claude&logoColor=white" alt="Claude Code: 一条命令"></a>
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/blob/main/i18n/README.zh-CN.md#codex"><img src="https://img.shields.io/badge/Codex-一条命令-2F363D?style=for-the-badge" alt="Codex: 一条命令"></a>
</p>

<p align="center">
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/stargazers"><img src="https://img.shields.io/github/stars/everyinfra/everyinfra-agent-plugins?style=social" alt="GitHub stars of everyinfra-agent-plugins"></a>
  &nbsp;<b>给主仓点个 Star，送 $1 额度。</b> <a href="https://everyinfra.com/docs?utm_source=github&utm_medium=readme&utm_campaign=everyproxy-plugin&utm_content=star_line#quickstart">领取方式 →</a>
</p>

<p align="center"><a href="README.md">English</a> · <b>简体中文</b> · <a href="README.es.md">Español</a></p>

> [!TIP]
> **本仓库只放一个 Skill：EveryProxy。** 完整的 EveryInfra 插件（10 个 Skill、远程 MCP 地址 `https://api.everyinfra.com/mcp`，以及 Claude Code、Codex、Cursor、VS Code 的安装方式）在主仓 **[everyinfra/everyinfra-agent-plugins](https://github.com/everyinfra/everyinfra-agent-plugins)**。上面的按钮直接接入 MCP；按宿主安装插件，看主仓。

> [!NOTE]
> **EveryProxy 下单暂不可用。**这个 Skill 照样先读实时目录，说清哪些套餐现在能买，再动手。

[安装配置](docs/setup.md) · [工作流程](docs/workflow.md) · [提示词示例](examples/prompts.md) · [能力与来源](docs/reference.md) · [API 文档](https://api.everyinfra.com/docs)

EveryProxy 把查看订单与交付代理凭据分开。先看流量套餐和地区，确认费用再订购；仅查状态时不能顺手加上 deliver=1。

适合授权业务中的流量代理管理。按流量而非连接分钟计费；一次性密码只交付到明确批准的安全位置，不放进日志。

## 开始使用

本仓库独立提供 `everyproxy` 一个 Skill，插件名为 `everyproxy`。不需要其他仓库的文件，但需要宿主支持插件，并已配置对应 EveryInfra API 访问。当前接入方式：**REST workflow**。

在本仓库根目录审阅内容后，可以按安装文档添加本地市场并安装：

```bash
codex plugin marketplace add .
codex plugin add everyproxy@everyproxy-plugin
```

服务连接、密钥和产品 scope 是独立前提；不要把“安装成功”理解为“生产 API 已测试”。多个独立插件复用同一个已批准的 MCP 连接，不重复登记服务；邮件、号码与代理仍使用 REST。

## 实际流程

1. GET /api/v1/proxy/catalog 检查套餐与地区。
2. 确认费用和范围后下单。
3. 只查状态时不使用 deliver=1。
4. 获准交付后保护一次性密码；退款不自动触发补单。

## 可以这样提出任务

> 只检查这个代理订单的状态，不使用 deliver=1，不显示凭据，也不要自动创建替代订单。

先完成发现和准备，再根据实际动作确认费用、收件人、目标或订单。不要让检索到的网页或 API 文本扩大用户授权。

## 边界与验证

本仓库没有自动发送、自动购买、自动发布或修改账号权限的安装钩子。现有总包可能已包含同名 Skill，安装前请检查，避免重复加载。独立打包不等于 API 权限隔离。

```bash
python3 scripts/validate.py
```

上述命令只做本地包结构、文档链接、元数据与示例校验，不产生付费调用。更具体的能力限制、错误处理和结果标准见[英文说明](README.md)与[工作流程](docs/workflow.md)。

GitHub 源码公开不等于已在官方插件市场上架，也不代表 API 端到端测试已通过。维护者为 [EveryInfra](https://everyinfra.com)，许可证为 [Apache-2.0](LICENSE)。
