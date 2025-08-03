# Ollama + Qwen + Tailscale Setup Guide

This guide documents the steps taken to set up Ollama with Qwen models on Windows with GPU acceleration, accessible via Tailscale.

## System Requirements
- Windows 10/11
- NVIDIA GPU (RTX 4070 Super with 12GB VRAM used in this setup)
- Administrator access (for firewall configuration)

## Step 1: Install Ollama

```powershell
winget install Ollama.Ollama --accept-source-agreements --accept-package-agreements
```

## Step 2: Configure Ollama for Network Access

Set the environment variable to allow network connections:

```powershell
# Set for current user (persistent)
[Environment]::SetEnvironmentVariable("OLLAMA_HOST", "0.0.0.0:11434", "User")

# Set for current session
set OLLAMA_HOST=0.0.0.0:11434
```

## Step 3: Start Ollama Service

```powershell
ollama serve
```

Verify it's running:
```powershell
curl http://localhost:11434/api/tags
```

## Step 4: Download Qwen Models

Download the recommended model for RTX 4070 Super:

```powershell
# Primary recommendation - Qwen 2.5 Coder 7B (specialized for coding)
ollama pull qwen2.5-coder:7b

# Alternative models
ollama pull qwen2.5:7b
ollama pull qwen2.5:14b  # Requires more VRAM
```

Verify installed models:
```powershell
ollama list
```

## Step 5: Test Models Locally

Quick test via API:
```powershell
curl -X POST http://localhost:11434/api/generate `
  -H "Content-Type: application/json" `
  -d '{"model": "qwen2.5-coder:7b", "prompt": "Say hello", "stream": false}'
```

Interactive test:
```powershell
ollama run qwen2.5-coder:7b
```

## Step 6: Install Tailscale

```powershell
winget install tailscale.tailscale --accept-source-agreements --accept-package-agreements
```

## Step 7: Configure Tailscale

Start Tailscale and authenticate:
```powershell
"C:\Program Files\Tailscale\tailscale.exe" up
```

Get your Tailscale IP:
```powershell
"C:\Program Files\Tailscale\tailscale.exe" ip -4
```

Check status:
```powershell
"C:\Program Files\Tailscale\tailscale.exe" status
```

## Step 8: Configure Windows Firewall

**Run as Administrator:**
```powershell
New-NetFirewallRule -DisplayName "Ollama" -Direction Inbound -LocalPort 11434 -Protocol TCP -Action Allow
```

## Step 9: Create Startup Script

Create `start-ollama-network.bat`:
```batch
@echo off
echo Starting Ollama with network access...
set OLLAMA_HOST=0.0.0.0:11434
ollama serve
```

## Step 10: Verify Remote Access

From any device on your Tailscale network:
```bash
curl http://[YOUR-TAILSCALE-IP]:11434/api/tags
```

## Useful Commands

### Monitor GPU Usage
```powershell
nvidia-smi -l 1
```

### Stop Ollama
```powershell
taskkill /F /IM ollama.exe
```

### Remove a Model
```powershell
ollama rm model-name
```

## Troubleshooting

1. **Port Already in Use**: Kill existing Ollama processes
   ```powershell
   Get-Process ollama | Stop-Process -Force
   ```

2. **Cannot Access Remotely**: 
   - Ensure OLLAMA_HOST is set to 0.0.0.0:11434
   - Check Windows Firewall rule is active
   - Verify Tailscale is connected

3. **Model Download Issues**: 
   - Check available disk space
   - Retry with `ollama pull model-name`

## Configuration Details

- **Ollama API**: http://localhost:11434
- **Remote Access**: http://[tailscale-ip]:11434
- **Models Location**: C:\Users\[username]\.ollama\models
- **GPU**: NVIDIA GeForce RTX 4070 SUPER (12GB VRAM)