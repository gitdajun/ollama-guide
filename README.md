# Ollama 安装与使用指南

在本地运行大语言模型（如 Qwen、DeepSeek、Llama、Gemma 等）。

- 项目：https://github.com/ollama/ollama
- 官网：https://ollama.com

## 安装

**Linux：**

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**macOS / Windows：** 从官网下载安装包。

## 常用命令

```bash
ollama run qwen2.5
ollama run deepseek-r1
ollama pull llama3.2
ollama list
ollama rm <模型名>
ollama serve
```

默认服务地址：`http://127.0.0.1:11434`，可被 Open WebUI 等前端调用。

## 注意

- 模型体积较大，预留足够磁盘与内存
- 有 GPU 可加速，无 GPU 时用 CPU 推理（更慢）

## 说明

请遵守当地法律与相关许可。使用风险自负。

## License

本仓库文档 MIT。
