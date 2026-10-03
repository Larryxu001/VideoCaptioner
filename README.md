<div align="center">
  <img src="./docs/images/logo.png" alt="VideoCaptioner Logo" width="100">
  <h1>VideoCaptioner</h1>
  <p>An LLM-powered video subtitle toolkit for speech recognition, subtitle optimization, translation, and video composition</p>

  [Online Documentation](https://weifeng2333.github.io/VideoCaptioner/) · [CLI Usage](#cli-command-line) · [Desktop GUI](#desktop-gui) · [Claude Code Skill](#claude-code-skill)
</div>

## Installation

```bash
pip install videocaptioner          # Install the CLI and desktop GUI
```

Free features (Bijian speech recognition and Bing/Google translation) **work immediately after installation, with no configuration required**.

## CLI Command Line

```bash
# Speech transcription (free, no API key required)
videocaptioner transcribe video.mp4 --asr bijian

# Subtitle translation (free Bing translation)
videocaptioner subtitle input.srt --translator bing --target-language en

# Full pipeline: transcribe → optimize → translate → compose
videocaptioner process video.mp4 --target-language ja

# Embed subtitles in a video
videocaptioner synthesize video.mp4 -s subtitle.srt

# Download online videos
videocaptioner download "https://youtube.com/watch?v=xxx"
```

To use LLM features (subtitle optimization and LLM translation), configure an API key:

```bash
videocaptioner config set llm.api_key <your-key>
videocaptioner config set llm.api_base https://api.openai.com/v1
videocaptioner config set llm.model gpt-4o-mini
```

Configuration precedence: `command-line arguments > environment variables (VIDEOCAPTIONER_*) > configuration file > defaults`. Run `videocaptioner config show` to view the current configuration.

<details>
<summary>All CLI Commands</summary>

| Command | Description |
|------|------|
| `gui` | Open the desktop GUI. You can also run `videocaptioner-gui` |
| `transcribe` | Transcribe speech to subtitles. Engines: `faster-whisper`, `whisper-api`, `bijian` (free), `jianying` (free), `whisper-cpp` |
| `subtitle` | Optimize/translate subtitles. Translation services: `llm`, `bing` (free), `google` (free) |
| `dub` | Generate a dubbed audio track or video from subtitles |
| `synthesize` | Embed subtitles in a video (soft or hard subtitles) |
| `process` | Run the full pipeline |
| `download` | Download videos from YouTube, Bilibili, and other platforms |
| `config` | Manage configuration (`show`, `set`, `get`, `path`, `init`) |

Run `videocaptioner <command> --help` for all options. See the full CLI documentation in [docs/cli.md](docs/cli.md).

</details>

## Desktop GUI

```bash
pip install videocaptioner
videocaptioner-gui                  # Explicitly open the desktop GUI
videocaptioner gui                  # Equivalent command
videocaptioner                      # Also opens the desktop GUI when run without arguments
```

<details>
<summary>Other Installation Options: Windows Installer / macOS One-Step Script</summary>

**Windows**: Download the installer from [Release](https://github.com/WEIFENG2333/VideoCaptioner/releases)

**macOS**:
```bash
curl -fsSL https://raw.githubusercontent.com/WEIFENG2333/VideoCaptioner/master/scripts/run.sh | bash
```

</details>


<!-- <div align="center">
  <img src="https://h1.appinn.me/file/1731487405884_main.png" alt="Interface preview" width="90%" style="border-radius: 5px;">
</div> -->

![Interface preview](https://h1.appinn.me/file/1731487410170_preview1.png)
![Interface preview](https://h1.appinn.me/file/1731487410832_preview2.png)

## LLM API Configuration

LLMs are used only for subtitle optimization and LLM translation. Free features (Bijian recognition and Bing translation) require no configuration.

All providers with OpenAI-compatible APIs are supported:

| Provider | Website |
|--------|------|
| **VideoCaptioner API Gateway** | [api.videocaptioner.cn](https://api.videocaptioner.cn) — High concurrency and cost-effective access to GPT, Claude, Gemini, and more |
| SiliconCloud | [cloud.siliconflow.cn](https://cloud.siliconflow.cn/i/HF95kaoz) |
| DeepSeek | [platform.deepseek.com](https://platform.deepseek.com) |

Enter the API Base URL and API key in the application settings or CLI. [Detailed Configuration Guide](https://weifeng2333.github.io/VideoCaptioner/config/llm)

## Claude Code Skill

This project provides a [Claude Code Skill](https://code.claude.com/docs/en/skills.md) that lets AI coding assistants use VideoCaptioner directly to process videos.

Install it in Claude Code:

```bash
mkdir -p ~/.claude/skills/videocaptioner
cp skills/SKILL.md ~/.claude/skills/videocaptioner/SKILL.md
```

Then enter `/videocaptioner transcribe video.mp4 --asr bijian` in Claude Code to use it.

## How It Works

```
Audio/video input → speech recognition → subtitle segmentation → LLM optimization → translation → video composition
```

- Word-level timestamps and voice activity detection (VAD) for accurate recognition
- LLM-based semantic segmentation for naturally readable subtitles
- Context-aware translation with reflection-based refinement
- Concurrent batch processing for higher efficiency

## Development

```bash
git clone https://github.com/WEIFENG2333/VideoCaptioner.git
cd VideoCaptioner
uv sync && uv run videocaptioner     # Run the GUI
uv run videocaptioner --help          # Run the CLI
uv run pyright                        # Type check
uv run pytest tests/test_cli/ -q      # Run tests
```

## License

[GPL-3.0](LICENSE)

[![Star History Chart](https://api.star-history.com/svg?repos=WEIFENG2333/VideoCaptioner&type=Date)](https://star-history.com/#WEIFENG2333/VideoCaptioner&Date)
