---
name: shannon
description: Shannon - AI 自主渗透测试工具，用于 Web 应用和 API 的白盒安全分析与漏洞利用验证
---

# Shannon - AI Pentester

## 概述

Shannon 是 Keygraph 开发的 AI 自主渗透测试工具。它通过白盒方式分析源代码，识别攻击向量，并使用浏览器自动化执行真实漏洞利用。只有具备可复现 PoC 的漏洞才会出现在最终报告中。

## 前置条件

- Docker 已安装并运行
- Anthropic API Key（Claude 模型）
- 被测试的 Web 应用源代码可访问

## 快速开始

### NPX 方式（零安装）

```bash
# 配置凭据（交互式向导）
npx @keygraph/shannon setup

# 直接导出环境变量（非交互式 / CI）
export ANTHROPIC_API_KEY=your-key

# 运行渗透测试
npx @keygraph/shannon start -u <url> -r /path/to/repo
```

### 本地方式（开发）

```bash
git clone https://github.com/KeygraphHQ/shannon.git
cd shannon
echo "ANTHROPIC_API_KEY=your-key" > .env
./shannon build
./shannon start -u <url> -r <repo-name>
```

## 常用命令

| 命令 | 说明 |
|------|------|
| `npx @keygraph/shannon setup` | 配置 AI 提供商凭据 |
| `./shannon start -u <url> -r <repo>` | 启动渗透测试 |
| `./shannon start -u <url> -r <repo> -w <name>` | 命名工作区（支持恢复）|
| `./shannon logs <workspace>` | 查看工作流日志 |
| `./shannon status` | 查看运行中的 worker |
| `./shannon workspaces` | 列出所有工作区 |
| `./shannon stop` | 停止（保留数据）|
| `./shannon stop --clean` | 完全清理 |
| `./shannon build` | 本地构建 Docker 镜像 |

## 选项

| 选项 | 说明 |
|------|------|
| `-u <url>` | 目标应用 URL |
| `-r <repo>` | 仓库路径 |
| `-c <file>` | YAML 配置文件 |
| `-o <path>` | 输出目录 |
| `-w <name>` | 命名工作区 |
| `--pipeline-testing` | 最小化提示 |
| `--debug` | 保留 worker 容器 |

## 漏洞覆盖

Shannon 当前覆盖以下 OWASP 漏洞类别：

- **注入攻击（Injection）**：SQL、命令、代码注入
- **跨站脚本（XSS）**：存储型、反射型、DOM 型
- **服务端请求伪造（SSRF）**
- **认证与授权缺陷**：认证绕过、权限提升

## 报告示例

- [OWASP Juice Shop 报告](https://github.com/KeygraphHQ/shannon/blob/main/sample-reports/shannon-report-juice-shop.md)
- [crAPI 报告](https://github.com/KeygraphHQ/shannon/blob/main/sample-reports/shannon-report-crapi.md)

## 架构

```
CLI (apps/cli/)     -> @keygraph/shannon npm 包，Docker 编排
Worker (apps/worker/) -> Temporal Worker，渗透测试管道逻辑
```

- CLI 负责 Docker Compose 生命周期、镜像拉取/构建、状态管理
- Worker 运行 Temporal 工作流，执行侦察、漏洞分析、利用和报告生成

## 重要说明

- **仅限白盒测试**：需要访问被测应用的源代码
- **仅限授权测试**：仅在你拥有或有权测试的系统上使用
- **模型支持**：官方仅支持 Claude 模型
- **耗时**：完整测试通常需 1-1.5 小时
- **费用**：使用 Claude Sonnet 约 $50 USD

## 相关资源

- GitHub: https://github.com/KeygraphHQ/shannon
- 官网: https://keygraph.io
- Discord: https://discord.gg/9ZqQPuhJB7
- Shannon Pro: https://keygraph.io
