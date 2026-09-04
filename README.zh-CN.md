# EveryProxy：流量代理订购与凭据交付

[English](README.md) · [安装配置](docs/setup.md) · [工作流程](docs/workflow.md) · [提示词示例](examples/prompts.md) · [能力与来源](docs/reference.md)

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
