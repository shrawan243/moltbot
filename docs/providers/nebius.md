---
summary: "Use Nebius models (Qwen3 Coder 480B) in Moltbot"
read_when:
  - You want Nebius models in Moltbot
  - You need Nebius setup guidance
---
# Nebius

Nebius provides access to high-performance AI models through their Token Factory API,
including the **Qwen3 Coder 480B** model optimized for coding tasks.

## Features

- **OpenAI-compatible API**: Standard `/v1` endpoints for easy integration
- **High-performance models**: Access to Qwen3 Coder 480B and other models
- **Streaming**: Supported on all models
- **Function calling**: Supported via OpenAI-compatible tool calls

## Setup

### 1. Get API Key

1. Sign up at [Nebius Console](https://console.nebius.com/)
2. Navigate to **Token Factory** and create an API key
3. Copy your API key

### 2. Configure Moltbot

**Option A: Environment Variable**

```bash
export NEBIUS_API_KEY="your-api-key"
```

**Option B: Interactive Setup (Recommended)**

```bash
moltbot onboard --auth-choice nebius-api-key
```

This will:
1. Prompt for your API key (or use existing `NEBIUS_API_KEY`)
2. Configure the provider automatically
3. Set the default model to `nebius/Qwen/Qwen3-Coder-480B-A35B-Instruct`

**Option C: Non-interactive**

```bash
moltbot onboard --non-interactive \
  --auth-choice nebius-api-key \
  --nebius-api-key "your-api-key"
```

### 3. Verify Setup

```bash
moltbot chat --model nebius/Qwen/Qwen3-Coder-480B-A35B-Instruct "Hello, are you working?"
```

## Configure via `moltbot configure`

1. Run `moltbot configure`
2. Select **Model/auth**
3. Choose **Nebius**

## Config file example

```json5
{
  env: { NEBIUS_API_KEY: "your-api-key" },
  agents: {
    defaults: {
      model: { primary: "nebius/Qwen/Qwen3-Coder-480B-A35B-Instruct" },
      models: {
        "nebius/Qwen/Qwen3-Coder-480B-A35B-Instruct": { alias: "Qwen3 Coder" }
      }
    }
  },
  models: {
    mode: "merge",
    providers: {
      nebius: {
        baseUrl: "https://api.tokenfactory.nebius.com/v1",
        apiKey: "${NEBIUS_API_KEY}",
        api: "openai-completions",
        models: [
          {
            id: "Qwen/Qwen3-Coder-480B-A35B-Instruct",
            name: "Qwen3 Coder 480B",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 262144,
            maxTokens: 32768
          }
        ]
      }
    }
  }
}
```

## Available Models

| Model ID | Name | Context (tokens) | Features |
|----------|------|------------------|----------|
| `Qwen/Qwen3-Coder-480B-A35B-Instruct` | Qwen3 Coder 480B | 262k | Coding |

## Usage Examples

```bash
# Use default Nebius model
moltbot chat --model nebius/Qwen/Qwen3-Coder-480B-A35B-Instruct

# Set as default model
moltbot models set nebius/Qwen/Qwen3-Coder-480B-A35B-Instruct

# List available models
moltbot models list | grep nebius
```

## Troubleshooting

### API key not recognized

```bash
echo $NEBIUS_API_KEY
moltbot models list | grep nebius
```

Ensure your API key is valid and the environment variable is set correctly.

### Connection issues

Nebius API is at `https://api.tokenfactory.nebius.com/v1`. Ensure your network
allows HTTPS connections.

## Links

- [Nebius Console](https://console.nebius.com/)
- [Nebius Documentation](https://docs.nebius.com/)
