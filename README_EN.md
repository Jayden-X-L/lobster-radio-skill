# Lobster Radio - Personalized AI News Radio Skill


A personalized AI news radio generation service powered by a local TTS model — completely free.

Built on Alibaba's open-source **Qwen3-TTS-12Hz 0.6B** local speech synthesis model, Lobster Radio delivers a fully automated pipeline: **Intelligent News Fetching → Content Refinement → Personalized Voice Synthesis**.

- News content is generated using OpenClaw's currently configured LLM model
- Audio synthesis runs entirely locally with zero API costs, supporting 9 professional voice presets
- Ideal for commute, workouts, housework, or any moment you want hands-free information

---

## Features

- 🎙️ **Personalized Radio Generation** — Generate audio news based on any topic or tag you choose
- ⏰ **Scheduled Delivery** — Set up daily auto-delivery to your preferred platform
- 🎨 **Multiple Voice Presets** — 9 built-in voices with emotion control
- 💰 **100% Free** — Local TTS model, no API fees, no usage limits
- 🐣 **Automatic Setup** — Skill auto-detects your environment and downloads models from global or Chinese CDNs
- 🔒 **Privacy First** — All processing happens locally, no data leaves your machine
- 📡 **Multi-Platform** — Supports Feishu, Telegram, WeChat, and 20+ other platforms

---

## System Requirements

Lobster Radio runs on virtually any modern computer.

### Minimum Requirements
- CPU: Any modern CPU
- RAM: 4GB
- Storage: 2GB free space

### Recommended Requirements
- GPU: NVIDIA GPU with CUDA support
- VRAM: 2GB+
- RAM: 8GB
- Storage: 5GB free space

---

## Installation

### Prerequisites

> **Important**: The Qwen3-TTS model is **not available in the Ollama public registry**. It must be downloaded from HuggingFace or ModelScope.

#### System Requirements for Qwen3-TTS

**Minimum (Qwen3-TTS-12Hz-0.6B-Base)**:
- CPU: Any modern CPU
- RAM: 8GB
- Storage: 5GB free space
- GPU: Optional (recommended)

**Recommended (Qwen3-TTS-12Hz-0.6B-Base)**:
- GPU: NVIDIA GPU with CUDA
- VRAM: 4GB+
- RAM: 16GB
- Storage: 10GB free space

### Method 1: One-Click Install (Recommended)

```bash
cd lobster-radio-skill
bash scripts/install.sh
```

The install script automatically:
- ✅ Installs Python dependencies
- ✅ Downloads the Qwen3-TTS model (on first use)
- ✅ Configures TTS
- ✅ Integrates with OpenClaw

### Method 2: Manual Install

#### 1. Install Python Dependencies

```bash
cd lobster-radio-skill
pip install -r requirements.txt
```

#### 2. Download the Qwen3-TTS Model

**Option A: From HuggingFace (Recommended)**

```bash
pip install huggingface_hub
huggingface-cli download Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice --local-dir ./models/Qwen3-TTS-12Hz-0.6B-Base
```

**Option B: From ModelScope (Recommended for China-based users)**

```bash
pip install modelscope
python -c "from modelscope import snapshot_download; snapshot_download('qwen/Qwen3-TTS-12Hz-0.6B-Base', cache_dir='./models')"
```

**Option C: Via Python Script**

```python
from huggingface_hub import snapshot_download

snapshot_download(
    repo_id="Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice",
    local_dir="./models/Qwen3-TTS-12Hz-0.6B-Base",
    local_dir_use_symlinks=False
)
```

#### 3. Verify Model Download

```bash
ls -lh ./models/Qwen3-TTS-12Hz-0.6B-Base
# Expected files:
# config.json, pytorch_model.bin, tokenizer.json, tokenizer_config.json
```

#### 4. Integrate with OpenClaw

**Option A: Copy to workspace**
```bash
cp -r lobster-radio-skill ~/.openclaw/workspace/skills/
openclaw gateway restart
```

**Option B: Symlink (recommended for development)**
```bash
ln -s $(pwd)/lobster-radio-skill ~/.openclaw/workspace/skills/lobster-radio-skill
openclaw gateway restart
```

#### 5. Verify Skill Installation

```bash
openclaw skills list
# You should see "lobster-radio" in the list
```

### Further Reading

- See [INSTALL.md](INSTALL.md) for detailed installation and configuration instructions
- See [QWEN3TTS_GUIDE.md](QWEN3TTS_GUIDE.md) for in-depth Qwen3-TTS model usage

---

## Usage

### Via OpenClaw Chat

On any platform supported by OpenClaw:

**Generate a radio:**
```
Generate a radio about artificial intelligence
```

**Set up scheduled delivery:**
```
Every morning at 8am, push a tech news radio
```

**Configure your voice:**
```
Set up my radio voice
```

### Via Command Line

**Generate a radio:**
```bash
python scripts/generate_radio.py --topics "artificial intelligence" --tags "tech"
```

**Configure TTS:**
```bash
python scripts/configure_tts.py --voice xiaoxiao
```

**List history:**
```bash
python scripts/list_radios.py
```

---

## Available Voices

### Chinese Voices
| Voice ID | Name | Description |
|---|---|---|
| **xiaoxiao** | Xiaoxiao | Female, warm, ideal for news |
| **yunjian** | Yunjian | Male, steady, great for finance |
| **xiaochen** | Xiaochen | Female, lively, perfect for entertainment |
| **xiaoyu** | Xiaoyu | Male, youthful, for tech content |
| **xiaoya** | Xiaoya | Female, articulate, suited for education |

### Emotion Settings
| Emotion | Best For |
|---|---|
| **neutral** | News broadcasts |
| **happy** | Entertainment content |
| **sad** | Serious topics |
| **excited** | Tech breakthroughs |

---

## Cowork Mode (Recommended)

In LobsterAI/OpenClaw's cowork mode, the platform's main LLM handles content generation while this Skill focuses solely on TTS synthesis.

**Works with any LLM**: Claude, GPT-4, Qwen, Llama, Gemini, and more.

**Advantages:**
- ✅ No extra LLM API keys needed
- ✅ Leverage the platform's full LLM capabilities
- ✅ Smarter, more natural content generation
- ✅ Switch between models freely for comparison

**Important: Fetching News Content**

Before generating a radio, you need to gather the latest news. **Call the web-search skill multiple times** to cover different topics:

> **Tip**: Keep your news script under **200 words** to keep audio under 1 minute — perfect for mobile listening.

> **Note**: The web-search skill may not be directly importable via Python. Try in this order:
> 1. **Try Python import first**: `from web_search import search`
> 2. **Fall back to bash**: `bash scripts/web_search.sh "search keyword"`

```python
# 1. Call web-search skill multiple times for different topics
#    (3-5 searches to cover all bases)
#    Try Python import first, then bash script if needed

# Search examples:
# - 1st search: "top international news today"
# - 2nd search: "latest tech news"
# - 3rd search: "top business news today"
# - 4th search: "sports highlights today"
# - 5th search: "entertainment热点 today"

# 2. The platform's main LLM synthesizes all search results into a coherent radio script
content = """
Welcome to today's news summary.

In international news... (from web-search results)

In technology... (from web-search results)

In finance... (from web-search results)

In sports... (from web-search results)

In entertainment... (from web-search results)

That's all for today's news. Thanks for listening.
"""

# 3. Call the Skill to synthesize audio
audio_url = cowork_generate(
    title="Today's News Summary",
    content=content,
    voice="xiaoxiao",  # female voice works well for news
    emotion="neutral"
)

print(f"Audio generated: {audio_url}")
```

**This Skill only supports Cowork Mode:**

| Mode | Content Gen | TTS | Extra API Key | Supported Models |
|---|---|---|---|---|
| **Cowork Mode** | **Platform LLM** | **Skill** | **None** | **Any** |

---

## Performance

| Metric | Value |
|---|---|
| **CPU inference** | 1–2 sec / 100 chars |
| **GPU inference** | 0.5–1 sec / 100 chars |
| **Memory usage** | ~500MB |
| **Model size** | ~600MB |

---

## Cost

- ✅ **100% free** — no API call fees
- ✅ **No usage limits** — unlimited generation
- ✅ **Privacy protected** — all local, nothing sent to the cloud
- ✅ **Works offline** — no internet required for synthesis

---

## Project Structure

```
lobster-radio-skill/
├── SKILL.md                 # Skill definition (OpenClaw)
├── skill.json               # Skill definition (LobsterAI)
├── requirements.txt         # Python dependencies
├── README.md                # This file (Chinese)
├── README_EN.md             # English version
├── __init__.py              # Package init
├── cowork_mode.py           # Cowork mode entry point
├── scripts/                 # Utility scripts
│   ├── generate_radio.py     # Radio generation
│   ├── configure_tts.py      # TTS configuration
│   ├── list_radios.py       # List radio history
│   └── install.sh           # One-click installer
├── providers/                # TTS provider implementations
│   ├── tts_base.py          # TTS base class
│   ├── qwen3_tts.py         # Qwen3-TTS local model
│   └── tts_factory.py       # TTS factory
├── utils/                    # Utility modules
│   ├── content_generator.py      # V1 content generator
│   ├── content_generator_v2.py   # V2 content generator (recommended)
│   ├── content_filter.py         # Content filter
│   ├── platform_adapter.py       # Platform adapter
│   ├── platform_news_searcher_v2.py  # News searcher
│   ├── audio_manager.py          # Audio file manager
│   └── config_manager.py         # Config manager
├── templates/                 # Prompt templates
│   └── prompts/
├── data/                     # Data directory
│   └── radios/                # Generated audio files
├── models/                   # TTS model files
└── tests/                    # Tests
    ├── test_skill.py
    └── verify_all.py
```

---

## Troubleshooting

### Model Not Downloaded

**Error**: "Model not found" or "Model download failed"

**Solutions**:
```bash
# Option 1: HuggingFace
pip install huggingface_hub
huggingface-cli download Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice --local-dir ./models/Qwen3-TTS-12Hz-0.6B-Base

# Option 2: ModelScope (for China-based users)
pip install modelscope
python -c "from modelscope import snapshot_download; snapshot_download('qwen/Qwen3-TTS-12Hz-0.6B-Base', cache_dir='./models')"

# Verify
python tests/verify_all.py
```

### Audio Generation Failed

**Error**: "Audio generation failed"

**Possible causes**:
1. Model not loaded correctly
2. Insufficient RAM/VRAM
3. Text too long

**Solutions**:
```bash
# Check model status
python tests/verify_all.py

# Use CPU mode if VRAM is insufficient
# Set use_gpu=False in config

# Check system resources
htop
```

---

## Development

### Set Up Dev Environment

```bash
git clone https://github.com/your-repo/lobster-radio-skill.git
cd lobster-radio-skill

python -m venv venv
source venv/bin/activate  # Linux/macOS
# or
venv\Scripts\activate  # Windows

pip install -r requirements.txt
```

### Run Tests

```bash
pytest tests/
```

---

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## License

MIT License — see [LICENSE](LICENSE) file.

---

## Support

For questions and issues:
- **GitHub Issues**: https://github.com/Jayden-X-L/lobster-radio-skill/issues
- **OpenClaw Docs**: https://docs.openclaw.ai
- **LobsterAI Docs**: https://github.com/netease-youdao/LobsterAI
- **Qwen3-TTS Docs**: https://qwen.ai/blog?id=qwen3tts-0115

---

## Acknowledgements

- **OpenClaw** — Powerful AI assistant platform
- **Qwen3-TTS** — Alibaba's open-source high-quality TTS model

---

*🦞 Lobster Radio — Information at your fingertips, news with warmth.*
