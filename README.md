# Ollama 详细安装与使用指南

Ollama 用于在本地拉取并运行大语言模型，通过命令行或 HTTP API 对话，数据默认留在本机。

- 源码：https://github.com/ollama/ollama
- 官网与模型库：https://ollama.com
- 默认 API：`http://127.0.0.1:11434`

---

## 一、安装

### Linux

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

安装后一般会注册系统服务。检查是否在跑：

```bash
ollama --version
curl -s http://127.0.0.1:11434/api/tags
```

### macOS

1. 打开 https://ollama.com/download 下载 macOS 安装包
2. 安装后菜单栏会出现 Ollama 图标
3. 终端执行 `ollama` 验证

### Windows

1. 官网下载 Windows 安装程序
2. 安装完成后在终端（PowerShell / CMD / Windows Terminal）使用 `ollama` 命令
3. 若命令未找到，检查是否已加入 PATH，或重开终端

---

## 二、核心命令

```bash
# 下载并进入交互对话（没有本地副本时会先 pull）
ollama run qwen2.5

# 只下载不进入对话
ollama pull llama3.2
ollama pull deepseek-r1

# 已安装模型列表
ollama list

# 删除模型（释放磁盘）
ollama rm qwen2.5

# 查看模型详细信息
ollama show qwen2.5

# 复制/改名
ollama cp qwen2.5 qwen-backup

# 仅启动服务（前台）
ollama serve
```

交互中常用：多行输入按说明结束；`/bye` 或 Ctrl+D 退出（以当前版本提示为准）。

---

## 三、选模型时怎么考虑

| 维度 | 说明 |
|------|------|
| 参数量 | 7B、14B、32B… 越大越强，也越吃内存/显存 |
| 量化 | 同系列会有不同体积标签，磁盘与速度差异大 |
| 用途 | 通用聊天、代码、推理（R1 类）等，看模型页说明 |
| 硬件 | 内存不够会换页卡顿甚至失败；宁可选小一档跑通再升级 |

模型文件通常在用户目录下（Linux/macOS 常见 `~/.ollama`，Windows 在用户目录的 Ollama 相关文件夹）。

---

## 四、API 调用示例

服务启动后可用 HTTP 调用（供脚本或其他前端使用）：

```bash
curl http://127.0.0.1:11434/api/generate -d '{
  "model": "qwen2.5",
  "prompt": "用三句话介绍一下自己",
  "stream": false
}'
```

聊天补全风格（Chat）：

```bash
curl http://127.0.0.1:11434/api/chat -d '{
  "model": "qwen2.5",
  "messages": [{"role": "user", "content": "你好"}],
  "stream": false
}'
```

---

## 五、与 Open WebUI 等搭配

1. 先保证 `ollama list` 有模型，且 `11434` 可访问
2. 安装 Open WebUI 后，在设置里填写 Ollama 地址
3. Docker 中的 WebUI 访问宿主机 Ollama 时，常用 `http://host.docker.internal:11434`（Linux 有时需额外加 host 映射）

---

## 六、性能与环境变量（进阶）

- 多卡、显存层数、代理等可通过环境变量调整（具体名称以当前官方文档为准）
- 需要监听非本机地址时，配置 `OLLAMA_HOST`（例如 `0.0.0.0:11434`），**不要轻易对公网开放**
- 公司网络需代理时，为拉取模型配置系统或环境代理

---

## 七、常见问题

| 问题 | 处理方向 |
|------|----------|
| `command not found` | 未安装、PATH 未刷新、服务未装好 |
| pull 很慢或失败 | 网络、代理、磁盘空间；可换时段或镜像策略（按你环境合法配置） |
| 运行极慢 | 模型过大、在用 CPU、内存不足 |
| 端口占用 | 其它程序占用 11434；改 HOST 或结束占用进程 |
| 权限错误 | Linux 下用户是否在正确组、目录权限是否可写 |

---

## 八、安全与合规

- 本地模型仍可能输出不当内容，需人工把关
- 对外提供 API 时加好防火墙与鉴权
- 请遵守模型许可与当地法律

## License

本仓库文档 MIT。Ollama 本体许可以官方为准。
