# LocalLama 🦙

A guide to setting up a personal AI server using Ollama, Qwen models, and Tailscale for remote access.

## Overview

Turn your Windows gaming PC into a powerful AI server that you can access from anywhere. This setup allows you to:

- Run large language models on dedicated GPU hardware
- Access your AI models from any device via Tailscale
- Save battery and resources on your primary work machine
- Keep your data private and secure

## Contents

- **[setup-guide.md](setup-guide.md)** - Step-by-step technical guide for setting up Ollama with Qwen models and Tailscale
- **[local-llm-blog.md](local-llm-blog.md)** - Blog post explaining the benefits and use cases of running your own AI server

## Quick Start

1. Install Ollama and configure for network access
2. Download Qwen 2.5 Coder model (optimized for coding tasks)
3. Install and configure Tailscale
4. Access your AI server from any device on your Tailscale network

## System Requirements

- Windows 10/11
- NVIDIA GPU with 8GB+ VRAM (tested on RTX 4070 Super)
- Tailscale account (free for personal use)

## Why This Setup?

- **Performance**: Dedicated GPU means faster responses than CPU-based inference
- **Privacy**: Your code and data never leave your network
- **Cost-effective**: One-time hardware cost vs ongoing API fees
- **Resource efficiency**: Keep your laptop cool and quiet

## Example Usage

From any device on your Tailscale network:

```bash
curl -X POST http://[YOUR-TAILSCALE-IP]:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5-coder:7b",
    "prompt": "Explain this Python code...",
    "stream": false
  }'
```

## Models Tested

- **Qwen 2.5 Coder 7B** - Excellent for coding tasks, runs smoothly on 12GB VRAM
- **DeepSeek R1 7B** - General purpose model

## License

This documentation is provided as-is for educational purposes.

## Author

Created by [@bdougie](https://github.com/bdougie)